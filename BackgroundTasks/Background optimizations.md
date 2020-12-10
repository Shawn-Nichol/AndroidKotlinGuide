# Background optimizations
Background processes can be memory and batter intensive. For exmple an implicit broadcast may start many background processes that have registered to listen for it, even if theose process may not do much work. This can have s ubstantial impact on both device perfromancce and user experince. 
To allevaite the issue Android 7.0 (API 24) applies the following restrictions
- Apps targeting Android 7.0 and higher do not receive CONNECTIVITY_ACTION broadcasts if they delcare their broadcast receiver in teh manifest. Apps will still receive CONNECTIVITY_ACTION broadcasts if they register their BroadcastReceiver with the COntext.registerReceiver() and that context is still valid. 
- Apps cannot ssend or receive ACTION_NEW_PICTURE or ACTION_NEW_VIDEO broadcasts. This optimization affects all apps, not only those targting Android 7.0

If your app uses any of these intents, you should remove dpendencies on them as soon as possible so that you can properly target devices running Android 7.0 or higher. The android famework provides everal solutions to mitigate the need for these implicit broadcassts. OR example JobScheduler and the new WorkManager provide robut mechanisms to shcdule network operations when spedified conditions such as a connectio an unmetereed network are met. You can now also use JobScheduler to react to chagne to content providers. JobInfo objects encapsulate the parameters that Jobscheudler uses to sehdule tyour job. When the conditions of the job ar met the system executes this job on your apps JobSErvice. 

## User-initiated restirctions
Beginning in Android 9 API 28, if an app exhibits some of the bad behaviors describe din Adnroid vitals, the system prompts the user to restrict the app's access to system resources. 

If the system notices that an app is consuming excessive resources, it notifies the user and gives the user th eoption of resticting the app's actions. Behaviors that can trigger the notifies the user and igves the user the option of restictin the app's actions. Behaviors that can trigger the notice include
- excessive  wake locks: 1 partial wake lock held for an hour when screen is off
- Excessive backgroun services: if app targets API level lower than 26 and has excessive background services 

The precise restrictions imposed are deteremined by the device manufactureer. For exmple on AOSP builds, restricted apps cannot run jobs, trigger alarms, or use the network except when the app is in the foregournd

## Restrictiosn on receivnig network activiity braodcasts
Apps targeting android 7.0 do not receiv eCONNECTIVITY_ACTION broadcasts if they register to receive them in their manifest, and processes that depend on this broadcast will not srat. This could pose a problem for apps that want to listen for network chagnes or perform bulk network activites when the device connects to an unmetereed network. Several solutions to get around this restriction already exist in teh Android famework, but choosing the righ one depends on what you want  your app to accomplish

## Schedule network jobs on unmetered connections 
When using the JobInfo.Builder class to build your Jobinfo jobject apply the sestRequireNetworkType() method and pass JobInfo.NETWORK_TYPE_UNMETERED as a job parameter. The followin gcode sample schdules a ssercie to run when the device connects to an unmetereed network and is chargin

```
const val MY_BACKGROUND_JOB = 0
...
fun scheduleJob(context: Context) {
    val jobScheduler = context.getSystemService(Context.JOB_SCHEDULER_SERVICE) as JobScheduler
    val job = JobInfo.Builder(
            MY_BACKGROUND_JOB,
            ComponentName(context, MyJobService::class.java)
    )
            .setRequiredNetworkType(JobInfo.NETWORK_TYPE_UNMETERED)
            .setRequiresCharging(true)
            .build()
    jobScheduler.schedule(job)
}
```

When the donditions ofr your job ar met, your app receives a callback to run the onstartJob() method in the specified Jobservice.class. To

A new alternative to Jobscheduler is workmanager, an ApI that allows yout to schedule background tasks that need guaranteed completion, regardless of whether the app process is around or not. Workmanager choose the appropriate way to run the work (either directly on a thread in your app process as well as using JobScheduler, FirebaseJobDispatcher, or AlarmManager) based on such factors as the device API level. Additionally, WorkMaanager does not require Play services and provides several advanced features, such as chaining tasks together or chcking a task's staus To lear 

## Monitor network connectivit while the app is running 
App that are running can still listen for Connectivity_change with a registered BroadcastReciever. However, the connectivity Manager API privdes a more robust method to request a callback only when specified newtork conditions are met. 

Network requests objects deine the parameters of the newtork callback in terms of Network capabilities. you create Network request objects with the NetworkRequest.Builder class. registerNetworkCallback() then passes the networkReuqest objects with the networkRequest object to the system. When the nework conditions are met, the app receives a callback to execute the onAvailable) method defined in its connectivity Manger. Networkcallback classs. 
Teh app continue sot receive callback until either the app exits or it calls unregisterNetworkCallaback(). 

## Restrictiosn on receiving image and video broadcasts
In android 7 (API 24) apps are nota ble to send or receive ACTION_NEW_PICTURE or ACTION_NEW_VIDEO broadcasts. This restiction helps alleviate the performance and user experince impacts when sevarl apps must wake up in order to prcess a new image or video. Android 7 extends Jobinfo and jobpramaeters to provide an alternative solution.

## Trigger jbos on ocntent URI changes 
To trigger jobs on content URI chagnes, Android 7 extends the JbInfo ApI with the follwoig methods 

`JobInfo.TriggerContentUri()`
Encapsulates parameters required to trigger a job on a ocntnent URI chagnes.

`JobInfo.Builder.addTriggerContentUri()`
Passes a triggerCOntentUri object ot JobInfo. A ContentObserver onitors the encapsulated content. URI. IF there are multple triggerContentUri objects associated with a job,the system provides a callback even if it reports a chagne in nonly one of the ocontent URIs. 

Add a the TriggerContentUri.FLAGNOTIFY_FOR_DESENDANTS flag to triggger the job if any descendants of the given URI chagne. This flag coresponds to the notify FOrDesendants parameter passed to registerContentOBserver

The following samle code schdules a job to trigger when the system reports a chagen to the conten t URI, Media URI
```
const val MY_BACKGROUND_JOB = 0
...
fun scheduleJob(context: Context) {
    val jobScheduler = context.getSystemService(Context.JOB_SCHEDULER_SERVICE) as JobScheduler
    val job = JobInfo.Builder(
            MY_BACKGROUND_JOB,
            ComponentName(context, MediaContentJob::class.java)
    )
            .addTriggerContentUri(
                    JobInfo.TriggerContentUri(
                            MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
                            JobInfo.TriggerContentUri.FLAG_NOTIFY_FOR_DESCENDANTS
                    )
            )
            .build()
    jobScheduler.schedule(job)
}
```

When the system reports a chagne in teh specified content URI, your appp receives a callback and a JobParameters object  is passed ot he onsTartJob() in `mediaContentJob.class`

## Determine which content authroities triggered a job
Android 7 also extends JobParameters to allow your app to receive useful information about what content authroities and URI triggered the job

URI getTriggeredContentURis()
returns an array of URIs that have triggered the job. This will be null if either no URIS have triggered the job (for exmaple, the job was triggered dueu to a deadline or some oher reason), or the number of chagned URIs is greater than 50. 

`String[] getTriggeredContentAuthorities()`
Returns a string array of content authroities that have triggered the job. If the returend array is not null, use getTriggeredContentUris(0 to retrieve the details of which URIs have chagned. 

The following sample code override the JobService.onStartJob() method and records the content authroities and URIs that have triggered the job. 
```
override fun onStartJob(params: JobParameters): Boolean {
    StringBuilder().apply {
        append("Media content has changed:\n")
        params.triggeredContentAuthorities?.also { authorities ->
            append("Authorities: ${authorities.joinToString(", ")}\n")
            append(params.triggeredContentUris?.joinToString("\n"))
        } ?: append("(No content)")
        Log.i(TAG, toString())
    }
    return true
}
```

## Further optimize your app 
Optimzing your app to un on low-memory devices, or in low memory conditions can improve perfomrance and user experince. Remvoign dpendinecies on bacckground services and manifest regsitered implicit broadacsst receiver can help your app run better on such devices. Although Android 7.0 takes steps to reduce some of these issues, it is recommended that you optimize your app to run witout the use of these background proccesse entirely. 

Android 7 introduces some addtional Android debug Bridge ADB commands that you can use to test app behavior with those backgroun processes disabled. 

To simualte conditions where implicit broadcasts and bacckground services are unavailable, enter the following commadn
```
$ adb shell cmd appops set <package_name> RUN_IN_BACKGROUND ignore
```
To reenable implicit broadcasts and background services, enter the following command
```
$ adb shell cmd appops set <package_name> RUN_IN_BACKGROUND allow
```
