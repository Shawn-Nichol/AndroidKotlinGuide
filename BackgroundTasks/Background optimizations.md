# Background optimizations
Background processes can be memory and battery intensive. For exmple an implicit broadcast may start many background processes that have registered to listen for it, even if those process may not do much work. This can have substantial impact on both device perfromance and user experince.

To allevaite the issue Android 7.0 (API 24) applies the following restrictions
- Apps targeting Android 7.0 and higher do not receive CONNECTIVITY_ACTION broadcasts if they delcare their broadcast receiver in the manifest. Apps will still receive CONNECTIVITY_ACTION broadcasts if they register their BroadcastReceiver with the Context.registerReceiver() and that context is still valid. 

- Apps cannot send or receive ACTION_NEW_PICTURE or ACTION_NEW_VIDEO broadcasts. This optimization affects all apps, not only those targting Android 7.0

`JobScheduler` and the `WorkManager` provide robuts mechanisms to schedule network operations when specified conditions such as a connection, an unmetered network are met. Use `JobScheduler` to react to changes to content providers. `JobInfo` objects encapsulate the parameters that `Jobscheudler` uses to schedule the job. When the conditions of the job are met the system executes this job on your apps `JobService`. 

## User-initiated restirctions
Beginning in Android 9 API 28, if an app exhibits some of the bad behaviors describe in Adnroid vitals, the system prompts the user to restrict the app's access to system resources. 

If the system notices that an app is consuming excessive resources, it notifies the user and gives the user the option of resticting the app's actions. Behaviors that can trigger the notification include. 

- Excessive wake locks: 1 partial wake lock held for an hour when screen is off
- Excessive backgroun services: if app targets API level lower than 26 and has excessive background services 

The restrictions imposed are deteremined by the device manufactureer. For exmple on AOSP builds, restricted apps cannot run jobs, trigger alarms, or use the network except when the app is in the foregournd

## Restrictiosn on receivnig network activity broadcasts
Apps targeting android 7.0 do not receive CONNECTIVITY_ACTION broadcasts if they register to receive them in their manifest, a processes that depend on this broadcast will not start. This could pose a problem for apps that want to listen for network chagnes or perform bulk network activites when the device connects to an unmetereed network. Several solutions to get around this restriction already exist in the Android famework. 

## Schedule network jobs on unmetered connections 
When using the `JobInfo.Builder` class to build your `Jobinfo` object apply the setRequireNetworkType() method and pass JobInfo.NETWORK_TYPE_UNMETERED as a job parameter. The following schdules a service to run when the device connects to an unmetereed network and is charging.

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

When the conditions of your job are met, your app receives a callback to run the `onStartJob()` method in the specified `Jobservice.class`.

A new alternative to `Jobscheduler` is `WorkManager`, an API that allows you to schedule background tasks that need guaranteed completion, regardless of whether the app process is around or not. `WorkManager` chooses the appropriate way to run the work (either directly on a thread in your app process as well as using JobScheduler, FirebaseJobDispatcher, or AlarmManager) based on such factors as the device API level. Additionally, `WorkManager` does not require Play Services and provides several advanced features, such as chaining tasks together or checking a task's status. 

## Monitor network connectivity while the app is running 
Apps that are running can still listen for `Connectivity_change` with a registered BroadcastReciever. However, the connectivity Manager API provides a more robust method to request a callback only when specified newtork conditions are met. 

Network requests objects define the parameters of the newtork callback in terms of Network capabilities. Create Network request objects with the `NetworkRequest.Builder` class. `registerNetworkCallback()` then passes the networkReuqest objects with the networkRequest object to the system. When the nework conditions are met, the app receives a callback to execute the `onAvailable()` method defined in its connectivity Manger. `NetworkCallback` classs. 
The app continues to receive callbacks until either the app exits or it calls `unregisterNetworkCallaback()`. 

## Restrictiosn on receiving image and video broadcasts
In android 7 (API 24) apps are not able to send or receive `ACTION_NEW_PICTURE` or `ACTION_NEW_VIDEO` broadcasts. This restiction helps alleviate the performance and user experince impacts when sevral apps must wake up in order to prcess a new image or video. Android 7 extends Jobinfo and jobpramaeters to provide an alternative solution.

## Trigger jbos on content URI changes 
To trigger jobs on content URI chagnes, Android 7 extends the JobInfo API with the follwoig methods 

`JobInfo.TriggerContentUri()`
Encapsulates parameters required to trigger a job on a ocntnent URI chagnes.

`JobInfo.Builder.addTriggerContentUri()`
Passes a triggerCOntentUri object ot JobInfo. A ContentObserver onitors the encapsulated content. URI. IF there are multple triggerContentUri objects associated with a job,the system provides a callback even if it reports a chagne in nonly one of the ocontent URIs. 

Add a the TriggerContentUri.FLAGNOTIFY_FOR_DESENDANTS flag to triggger the job if any descendants of the given URI chagne. This flag coresponds to the notify FOrDesendants parameter passed to registerContentOBserver

The following sample code schedules a job to trigger when the system reports a change to the content URI, Media URI
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

When the system reports a change in the specified content URI, your app receives a callback and a JobParameters object  is passed to the `onStartJob()` in `mediaContentJob.class`

## Determine which content authroities triggered a job
Android 7 also extends JobParameters to allow your app to receive useful information about what content authroities and URI triggered the job

URI getTriggeredContentURis()
returns an array of URIs that have triggered the job. This will be null if either no URIS have triggered the job (for exmaple, the job was triggered dueu to a deadline or some oher reason), or the number of chagned URIs is greater than 50. 

`String[] getTriggeredContentAuthorities()`
Returns a string array of content authroities that have triggered the job. If the returend array is not null, use `getTriggeredContentUris()` to retrieve the details of which URIs have chagned. 

The following sample code override the `JobService.onStartJob()` method and records the content authroities and URIs that have triggered the job. 
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
