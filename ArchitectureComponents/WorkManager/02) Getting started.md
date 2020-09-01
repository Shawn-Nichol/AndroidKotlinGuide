# Getting started
Provides intructions on basic setup

## Add dependices
```
  def work_version = "2.4.0"
    // Kotlin + coroutines
    implementation "androidx.work:work-runtime-ktx:$work_version"
```

## Worker class
Work is defined using the Worker class. The doWork() method is run synchronously  on a background thread provided by WorkManager. To create some work for the WorkManager, extend the Worker class and override doWork()
```
class MyWorker(context: Context, params: WorkerParameters) : Worker(context, params) {
  
  override fun doWork(): Result {
    // Do the work here. 
    
    resturn result.success()
  }
}
```
The result returned from dowWork() informs the WorkManager service whether the work succeeded, failed, or whether or not he work should be retried.
- Result.success(): The work finished successfully
- Result.failure(): The work failed
- Result.retry(): the work failed and should be tried at another time according to its retry policy

## Create a WorkRequest
Work is defined in WorkManager via a WorkRequest. The Work request object contains all the information needed by WorkManager to schedule and run work. WorkRequest includes constraints which must be met, scheduling information suchs as dealsy or repeating intervals, retry configuration, an dmay includ input data. Once your WorkRequest is defined it must be scheduled with the WorkManager service in order for it to run. 

WorkRequest itself is an abstract base class. There are two derived implementations of this class that you can use to create the request OneTimeWorkRequest and PeriodicWorkRequest

### OneTimeWorkRequest
This code WorkRequest will run once. 
```
 val myWorkRequest: WorkRequest = OneTimeWorkRequestBuilder<MyWorker>().build()
```

### PeriodicWorkRequest
This work Request will once every 15 mintues. The interval period is defined as the minium  time between repeptions. The exact time that the worker is going to be executed depends on the constraints that you are using in your WorkRequest object and on the optimization sperformed by the system. 
```
val workRequest = PeriodicWorkRequestBuilder<SaveImageToFileWorker>(1, TimeUnit.HOURS).build()
```

### Work Constraints
Constraints ensure that work is deffered until optimal conditions are met. 
- NetworkType: Constraints the type of network required for your work to run
- BatteryNotLow: When set to true, your work willnot run if hte device i sin low battery mode
- RequiresCharging: When sest to true, your work will only run when dthe device is charging. 
- DeviceIdel: When set to true, this requires the user's device to be idle before the work will run. THis can be useful for running batched operatioins that might other wise have a negative performance impact on other apps running actively on the user's device. 
- StorageNotLow: When set to true, your work will not run if theuser's storage space on teh device is too low. 

To Create a set of constraints and associated them with the WorkRequest, create a Constraints instance using the COnstraints.Builder() and assign it to your WorkRequest.Builder()
```
val constraints = Constraints.Builder()
  .setRequiredNetworkType(NetworkType.UNMETERED)
  .setRequiresCharing(true)
  .build
  
val workRequest: WorkRequest = 
  OneTimeWorkRequesteBuilder<MyWork>()
    .setConstraints(constraints)
    .build()
```

When multiple constraints are specified, your work will run only when all the constraints are met. In the event that a constaints becomes unmet while your work is running, WorkManager will stop your worker. The work will then be retried when all teh constraints are met. 

### Delayed Work
If you do not want to run your work immediatley, you can specify your work to start after a minimum initial delay. 
```
val workRequest = OneTimeWorkRequetBuilder<MyWork>()
  .setInitialDelay(10, TimeUnit.MINUTES)
  .build()
```

### Retry and Backoff
If you require the WorkManager to retry your work, you can return Result.retry() form your worker. You work is then rescheduled according to a backoff delay and backoff policy. 

- Backoff delay specifies the mimmium amount of time to wait before tetrying your work after the first attempt. This value can be no less than 10 seconds. 
- Backoff policy defines how the backoff delay should increase over time for subsequent retry attempts. WorkManager supports 2 backoff policies, LINEAR and EXPONNENTIAL. 

Every work request has a backoff policy and bakcoff dleay. The default policy is EXPONENTIAL with a delay of 10 seocnds, but you canoverride this in WorkRequest. 

```
val myWorkRequest = OneTimeWorkRequestBUilder<MyWork>()
  .setBackoffCriteria( 
    BackoffPolicy.LINEAR, 
    OneTiemWorkRequest.MIN_BACKOFF_MILLIS,
    TimeUnit.MILLISECONDS)
  .build()
```

### Tag Work
Every work request has a unique identifier, which can be used to identify that work later in order to cancel the work or observe its progress.
```
val workRequest = OneTimeWorkRequestBuilder<MyWork>()
  .addTag("cleanup")
  .build()
```

### Assign input data
Input values are stored as key-value pairs in a Data object and can be set in WorkRequest. The Worker class can access teh input arguments by calling Worker.getINputData().
```
// Define the Worker requiring input
class UploadWork(appContext: Context, workerParams: WorkerParameters)
   : Worker(appContext, workerParams) {

   override fun doWork(): Result {
       val imageUriInput =
           inputData.getString("IMAGE_URI") ?: return Result.failure()

       uploadFile(imageUriInput)
       return Result.success()
   }
   ...
}


val myUploadWork = OneTimeWorkRequestBuilder<UploadWork>()
   .setInputData(workDataOf(
       "IMAGE_URI" to "http://..."
   ))
   .build()
```

## Workmanager
The last step is to submit the WorkRequest to WorkManager.  The exact time that worker is going to be executed depenends on the constraints that are used in the WorkRequest and on system optimization. WorkManager is desgned to give the best behavior under these restrictions

```
WorkManager
  .getInstance(myContext)
  .enqueue(myWorkRequest)
```
