# WorkManager
WorkManager is an API that makes it easy to schedule deferrable, asynchronous tasks that are expected to run even if the app exits or the device restarts. The WorkManager API is a suitable and recommended replacement for all previous Android backgroun scheduling APIs(Job Scheduler). Work Manager incorporates the features of its predecessors in a modern, consistent API that works back to API level 14 while also being conscious of batter life. 

## Work Constraints
Declaratively dfine the optimal conditions for your work to run using Work Constraints. For example, run only when the device is on Wi-Fi, when the device idle, or when it has sufficient storage space. 

## Robust Scheduling 
WorkManager allows you to schedule owrk to run one-time or repeatedly using flexible scheduling windows. Work can be tagged and named as well, allowign you to schedule unique, replaceable, work and monitor or cancel groups of work together. Scheduled work is sotred in an interannlly managedSQLite database and WorkManagaer takes care of ensuring that this work persists and is recheduled across device reboots. In addition, WorkManager adheres to power-saving features an best practices like Doze Mode, so you don't have to worry about it. 

## Flexibility Retry Policy
Sometimes work fails. WorkManager offers flexible retry policies, including a configurable exponential backoff policy

## Work Chaining
For complex related work, chain individual work tast toether usinga fluent, natural interface tath allows you to control which pieces run sequentially and which run in parallel. For each work task you can define  input and output data for that work. When chaining work tegether, WorkManager automatically passess output data from one work task to the next. 

```
WorkManager.getInstnac(...) 
  .beginWith(listOf(workA, workB)
  .then(workC)
  .enqueue()
```

## Build-in Threading Interoperability
WorkManager integrates seamlessly with RXJava and Coroutines and provides the flexibility to plug in your own asynchronous APis. 

## Use WorkManager for Deferrable and Reliable Work
WorkManager is inteneded for work that is deferrable- that is not required to run immediately and reuqired to run reliable even if the app exits or the device restarts
- Sendign Logs or analytics to backend services
- Periodicallly syncing application data with a server

WorkManager is not intended for in-process backgroudn work that can safely be terminated if the app process goes away or for twork that requires immediate execution. Please review the background process guide to see which solution meets your needds.


## Getting started with Workmanager

Add the following dependicies
```
  def work_version = "2.4.0"
    // Kotlin + coroutines
    implementation "androidx.work:work-runtime-ktx:$work_version"
```

Work is defined using the Worker class. The doWork() method is run synchronously on a background thread provided by WorkManager. To create some work for WorkManager to run, extend the Worker class and override the doWork().

```
class uploadWorker(appContext: Context, workerParams: WorkerParamaeters): Worker(appContext, workerParams) {
  override fun doWork(): Result {
    // Do the work here in this case upload the image
    uploadImages()
    
    // indicate whether the owrk finished succesfully with the Result
    return Result.sucess()
  }

```

The Result returned from doWork() informs the Workmanaager service whether the work succeeded and, in the case of failure, whether or not the work should be retried
- Result.success(): the work finished successfully
- Result.failure(): The work failed
- Result.retry(): the work failed and should be tried at another time according to its retry policy

## Create a WorkRequest
Once Your work is defined it must be scheduled with the WorkManager service in order to run. WorkManager offers a lot of flexibility in how you schedule your work. You can schedule it to run periodically overa interval of time, or you can schedule it ot run only one time. 

However you choose to schedule the work, you will always use a WorkRequestWhile a Worker defines the unit of work, a WorkRequest(and its subclasses) define how and when it should be run. In the simplest case, you can use a OneTimeRequest. 
```
val uploadWorkRequest: WorkRequest = OneTimeRequestBuiulder<UploadWorker>().build()
```

## Submit the WorkRequest to WorkManager using the enqueue() method.
```
WorkManager
  .getInstance(myContext)
  .enqueue(uploadWorkRequest)
```

The exact time that the worker is going to be executed depends on the constraints that are used in your WorkRequest and on system optimizations. WorkManager is designed to give the best behavior under these restrictions.

## Next Steps
WorkRequest can also includ additional information, such as the constrainst under which the work should run, input to the work, adleay and backoff policy for retrying work. In 

# Define work request
## Overrivew
Work is defined in WorkManager via a WorkRequest. In order to schedule any work with WorkManager you must first create a WorkRequest object and then enqueue it. 

```
val myWorkRequest = ..
WorkManager.getInstance(myContext.enqueue(myWorkRequest)
```
The WorkRequest object contains all o fthe information needed by Workmanager to schedule and run your work. It includes constraints which must be met for your work to run, scheduling infomration such as delays or repeating intervals, retry configuartion, and may include input data if your work relies on it. 

WorkRequeste itself is abastract base class. There are two derived implementations of this class that you can use to create the request, OnteTimeWorkRequest and PeriodicWorkRequest. As their namesimply OntiemWorkRequest is useful for scheduling non-repeating work, whilst PeriodicWorkRequest is moreappropriate for scheduling work that repeats on some interval

## Schedule one-time work
Simple
```
val myWorkRequest = OneTimeWorkRequest.from(MyWork::class.java)
```
more complex
```
val uploadWorkRequest: WorkRequest = OneTimeWorkRequestBuilder<MyWork>()
// additional confiuration
.build()
```

## Scheudle periodic work
Your app may at times require that certain work runs periodically. For example, you may want to periodically backup your data, download fresh content in your app, or upload logs to a server. 
```
val saveRequest = PeriodicWorkRequestBuilder<SaveImageToFileWorker>(1, TimeUnit.HOURS)
// additional conifuration
.build()
```
In this example work is scheduled with a one hour interval

The interval period is deinfed as the minimum time between repretitions. The exact time that the worker is going to be executed dependds on teh constraints that you are uisngin your WorkRequest object and on the optimization performed by the system. The minmum request is 15 minutes

## Flexible run intervals
If the nature of your work makes it sensitive to run timing, you can configure your PeriodicWorkRequest to run within a flexperiod inside each interval period. To define period ic work with a flex period, you pass a flexInterval along with the repeat Interval when creating the PeriodicWorkRequest. The flex period begins at repeatInterval - flexInterval, and goes to the end of the interval 

The following is an example of periodic work that can run during the last 15 mintues of every one hour. 
```
val myUploadWork = PeriodicWorkRequestBuilder<SaveImageToFileWorker>(
  1, TimeUnit.HOURS, // repeatInterval 
  15, TimeUnit.MIINUTES) // flexInterval
  .build()
```
The repeat interval must be greater than or equal to PeriodicWorkRequest.MIN_PERIODIC_INTERVAL_MILLIS

### Effect of Constraints on Periodic Work
You can apply constraints to periodic work. For example, you coudl add a constraint to your work request such that the work only runs when th euser's device is chargin. In this case, even if the defined reqpeat interval passes, the PeriodicWorkRequest willl not run until this condition is met. This could cause a particular run of your work to be delayed, or even skpped if the conditions are not met within the run interval

## Work constraints
Constraints ensure that work is deferred until optimal conditions are met. The following constraints are available to Workmanager. 

- NetworkType: Constraints that the type of network requried for your work to run. WIFI
- BatteryNotLow: When set to true, your work will not run if the device is in low battery mode.
- RequiresCharging: When set to true, your work will only run when the device is chargin
- DeviceIdle: When set to ture, this reuqires the user's device to be idle before the work will run. This can be useful for running batched operations that might othersise have a negative performance impact on other apps running actively on the user's device
- StorageNotLow: When set to true, your work will not run if the users's storage space on the device is too low. 

To create a set of constraints an associate it with some work, createa Constraints instance using the Constraints.Builder() and assign it to your WorkRequest.Builder().

Fore example, the following code builds a work request which only runs when the user's device is both charging and on Wi-Fi.
```
val constraints = Constraints.Builder() 
  .setRequiredNetworkType(NetworkType.UNMETERED)
  .setRequiresCharing(true)
  .build()

val myWorkRequest: WorkRequest = OneTimeWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
  .setConstraints(constraints)
  .build()
```
When multiple constrainst are specified, your work will run only when all the constraints are met. In the event that a constraint isn't met well your work is running, WorkMager will stop your worker. THe work will then be retired whend all the constraints are met. 

##  Delayed Work In
In the even that your wokr has  no constaints or that all the constraints are met when your work is enqueued, the sytem may choose to run the owrk immediatlely. if you do not want the work to be run immediately, you can specify your work to sratar after a minium initial delay

Here is an exampleof how yot set your work to run at least 10 minutes after it has been enqueued.
```
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
  .setInitialDelay(10, TimeUnites, MINUTES)
  .build()
```


## Retry and backoff Policy
If you require that WorkManager retyr your work, you can return Result.retry() from your worker. Your work is then rescheduled according to a backoffdelay  and backoffpolicy. 
- Backoff delay: Specifies the minimum amount of time to wait before retrying your work after the first attempt. This value can be no less than 10 seconds(or MIN_BACKOFF_MILLIS).
- Backoffpolicy: Defines how the backoff delay should increase over time for subsequent retry attempts. WorkManager supports 2 backoff policies, LINEAR and EXPONENTIAL. 
Every work request hasa backoff policy and backoff delay. THe default policy ois EXPONENTIAL with a delay of 10 seconds, but you can override this in your work request configuration. 

```
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
  .setBackoffCriteria(
    BackoffPolicy.LINEAR,
    oneTimeWorkRequest.MIN_BACKOFF_MILLIS,
    TimeUnit.MILLLISECONDS)
  .build()
```
In this example the mimum bakcoff dleay is set to the minimum allowed value, 10 seconds. Since the plicy is lINEAR th retry interval will increase by approximately 10 seconds with each new attempt. For instance the firs run finishes with Resulot.retry() will be attempeted again after 10 subsequent attempts. If the backoff policy were set to EXPONENTIAL, the retry duration sequence would be closer to 20 40 80 and so on. 

## Tag Work
Every work request has a unique identifier, which can be used to identify that work later in order to cancel the work or observe it progress. If you have a group of logically related work, you may also find it helpful to tag those work items. Tagging allows you to operate witha group of work requests together. 

For example, WorkManager.cancelAllWorkByTag(string cancels all Work requests with a particualr tag, and WorkManager.getWorkInfoByTag(string) returns a list of wthe WorkInfo objects which can be used to determine the current work state. 

```
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>() 
  .addTag("cleanup")
  .build()
```
Finally multiple tags can be added to a single work request. Interanlly thsee tags are stored as a set of string. From a work Request you retrieve its set of tags via WorkRequest.getTags().

## Asisign input data
Your work may require input data in order to do its work. For example, work that handles uploading an image might require the URI of the image to be uploaded as input. 

Input values are stored as key-value pairs in a Data object and can be set on teh work request. WorkManager will deliver the input data to your work when it executes the work. The Worker class can access teh input arguments by calling Worker.getINputData(). The code below shows how you can create a Worker instance which requires input data nad how to send it in your work request
```
// Define the Worker requiring input
class UploadWork(appContext: Context, workerParams: WorkerParameters) : 
  Worker(appContext, workerParmas) {
  
  override fun doWork(): result {
    val imageUriInput = inputData.getStrin("IMAGE_URI") ?: return Result.failure()
    
    uploadFile(imageUriInput)
    reutnr Result.success()
  }
  ...
}

// Create a WorkRequest for your Worker and sending it input
val myUploadWork = OneTimeWorkRequestBuilder<UploadWork>()
  .setInputData(workDataOf("IMAGE_URI" TO "HTTP://..."))
  .build()
```

Similarly, the data dlass can be used to output a return value. Input andoutput data are covered in more detail in the section input parameters and returned values. 
