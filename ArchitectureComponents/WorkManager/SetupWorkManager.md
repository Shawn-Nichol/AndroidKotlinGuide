# How to setup WorkManger
In order to start a WorkManager you will need to do the following

Add: dependices: Add gradle dependencies
Create WorkerClass: the work will be run in the doWork() method
Create WorkRequest: handles information about the Work and what type of WorkRequest.
Constrataints: work is deffered until conditions are met.
Data: information you can pass onto the worker.
Tag: Uniquie Identifier
Retry and BackOff: parameters
WorkManager: Passes the WorkRequest off to the JobScheduler.
UniqueWork: guarntess there is only one instance of work with a particular name.
ConflicResolution: Tell WorkManager what todo if there is a conflict
ChainingWork: Links work together.


## Add the dependcies
```
  def work_version = "2.4.0"
    // Kotlin + coroutines
    implementation "androidx.work:work-runtime-ktx:$work_version"
```

## Work Class
Work is defined using the worker class. The doWork() method funs synchronously on a background thread provided by WorkManager. 

doWork() can return three options
- Result.sucess(): The work finsished successfully
- Result.failure(): The work failed
- Result.retry(): The work failed and should be tried at another time. 

```
class MyWorker(context: Context, params: WorkerParameters) : Worker(context, params) {
  
  override fun doWokr() {
    // Do the work here. 
    
    return Result.success()
  }
}
```


## WorkReuqest
The WorkRequest object contains all the information needed by WorkManager to schedule and run work. WorkRequest also includes constraints and data for the WorkManager.

`Schedule`
```
val myWorkRequest = OneTimeWorkRequestBuilder<MyWorker>().build()
val myWorkRequest = PeriodicWOrkRequesetBuilder<MyWorker>(1, TimeUnits.HOURS).build()
```

`WorkConstraints`
Constraints ensure that work is deffered until optimal conditions are met.
- NetworkType: The type of network required to run work.
- BatteryNotLow: Work will not run if the battery is low. 
- RequiresChargining: When set to true, your work will only run when the device is charging.
- DeviceIdle: Requires the device to be idle before the work will run.
- StorageNotLow: Work will not run if the user's storage space on the device is too low. 

```
val constraints = Constraints.Builder()
  .setRequiredNetworkType(NetworkType.UNMETERED)
  .setRequiresChargin(true)
  .build
  
val workRequest = OneTimeWorkRequestBuilder<MyWork>()
  .setConstraints(constraints)
  .build()
```

`DelayWork`
If yo do not wan to run your work immediatley, you can specify your work to start after a mimum initial delay.
```
val workRequest = OneTimeWorkRequeset<MyWork()
  .setInitialDelay(10, TimeUnit.MINUTES)
  .build()
```

`Retry and Backoff`
The amount to wait before retrying the work. 
```
val myWorkRequest = OneTimeWorkRequestBuilder<MyWork>()
  .setBackOffCriteria(
    BackOffPolicy.LINEAR, 
    OneTimeWorkREquest.MIN_BACKOFF_MILLIS,
    TimeUnit.MILLSECONDS)
  .build()
```

`Tag work`
unique identifier, whcih can be used to identify the work later and cancel or observe it. 
```
val WorkRequest = OneTimeWorkRequestBuilder<MyWork>()
  .addTag("MyWorkerTag")
  .build()
```

`Data`
Pass data to the doWork() method.
```
val myData = Data.Builder()
  .putString("KEY_INPUT", "This is the vaule")
  .build
  
val workRequest = OneTimeWorkRequestBuilder<MyWorker>()
  .setInputData(myData)
  .build()
```
