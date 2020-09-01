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
  .enqueue(workRequest)
```

### Unique Work
Unique work guarantess that you only have one instance of work with a particular name at a time. Unique names are human readable and specified by the developer instead of being auto-generated by WorkManager. Unlike tags, unique names are only associated with a singlle instance of work

Unique work can be applied to one-time and periodic work. You can createa unique work sequence by calling one of thesee methods, dpending on whether you're scheduling repeating work one-time work. 
- WorkManager.enqueueUniqueWork: one-time
- WorkManager.enqueuUniquePeriodicWork(): for periodic work

Both methods acccept 3 arugments
- uniqueWorkName: a string used to uniquely identify the work request
- existingWorkPolicy: a enum which tells WorkManger what to do it there's already an unfinsihed chain of work with that unique name. 
- work: the WorRequest to schedule

```
val sendLogsWorkRequest = 
  PeriodicWorkRequestBUilder<SendLogsWorker>(24, TimeUnit.HOURS)
    .setCOnstraints(constraints)
    .build()
    
WorkManager.getInstance(this).enqueuUniquePeriiodicWork(
  "sendLogs"
  ExistingPeriodicWorkPolicy.KEEP,
  sendLogsWorkRequest
)
```
If the code runs while a sendLogs job is already in the queue, the existin gjob is kept and no new job is added. Unique work sequences can also be useful if you ened to gradually build up a long chain of tasks. For example  a photo editing app might let users undo a long chain of actions. Each of those undo operatiion might take a while, but they have to be performed in teh correc order. In this case, the app could create an undo chain and append each undo operation to the chain as neede

###Conflit resolution 
When scheduling unique work, you must tell Workmanager what action to take when there is a conflict. You do this by passing an enum when enquing the work. 

For one-time work, you provide an Existing WOrkPolicy, which support 4 options for hanlding the conflict. 
- Replace: existing work with the new work, this cancels the existing work. 
- Keep: existing work and ignore the new work. 
- Append, the new work to the end of the existin work. This policy will cause your new work to be chained to the existin gwork, running after the existing work finishes. 

The existing work becomes a prerequisite to the new work. If the existing work becomes CANCELLED or FAILED, the new work is also CANCELLED or FAILED. If you want the new work to run regardless of the staus of the existing work, use APPEND_ORrEPLACE instead. 
- APPEND_OR_REPLACE function similarly to APPEND, except that it is not dependent on prerequisite work status. If the existing work is CANCELLED or FAILED the new work still runs. 

For Periodic work, you provide an ExistingPeriodicWorkPolicy, which support 2 options REPLACE and KEEP. These options function the same as their ExistingWorkPolicy ocunterparts. 

### Observing your work
At any point after enqueuing work, you can check its status by quering WorkManager by its name, id or by tag associated with it. 

```
// by id
workManager.getWorkInfoById(syncWorker.id) // ListenableFuture<WorkInfo>

// by name
workManager.getWorkInfosForUniqueWork("sync") // ListenableFuture<List<WorkInfo>>

// by tag
workManager.getWorkInfosByTag("syncTag") // ListenableFuture<List<WorkInfo>>
```

The query returns a ListenableFuture of WorkInfo boject, which includes hte id of the work, its tags, its current State, and ay output data set via Result.success(outputData). AliveData  variant of each of the methods allows you to observe changes to the WorkInfo by registering a listener. For example, if youwanted to display a message to the user when some work finishes successfully, you could set it up as follows: 

```
workManager.getWorkInfoByIdLiveData(syncWorker.id)
    .observe(viewLifecycleOwner) { workInfo -> 
  if(workInfo?.state == WorkInfo.State.SUCCEEDED) {
    Snackbar.make(requireView(),
    "Completed, Snackbar.LENGTH_SHORT)
      .SHOW
  }
}
```

### Complex Work queries
How you can find all work the tag "syncTag", that is in the FAILED or CANCELLED state and has a unique work anme of either "preProcess" or "sync".
```
val workQuery = WorkQuery.Builder
    .fromTags(listOf("syncTag"))
    .addStates(listOf(WorkInfo.State.FAILED, WorkInfo.State.CANCELLED))
    .addUniqueWorkNames(listOf("preProcess", "sync")
  )
  .build()

val workInfos: ListenableFuture<List<WorkInfo>> = workManager.getWorkInfos(workQuery)
```

### Canceling and stopping Work
If you no longer need your previously enqueued work to run, you can ask for it to be cancelled. Work can be cancelled by its name, id  or by a tag. 
```

// by id
workManager.cancelWorkById(syncWorker.id)

// by name
workManager.cancelUniqueWork("sync")

// by tag
workManager.cancelAllWorkByTag("syncTag")
```
Under the hood, WorkManager checks the state of the work. If the work is already finished, nothing happends. Otherwise the work's state is changed to cancelled and the work  will not run in teh future. Any WorkREquest jobs that are dependent on this work will also be CANCELLED.

Currently RUNNING work receives a call to ListenableWorker.onStopped(). Override this method to handle any potenial cleanup. 


## Stop a running Worker
- You explicilty asked for it be cancelled( WorkManager.cancelWorkById(UUID)
- In the case of unique work, you explicitly enqueued a new WorkrRequest with an ExistingWorkPolicy of REPLACE. The old WorkRequest is immediately consdered cancelled. 
- Your work's constaints are no longer met
- The system instructed your app to sop your work for some reason. This can happen i fyou exceed the execution deadline of 10 minutes. The work is scheduled for retry at a later time. 

Under these conditions, your worker is stopped you Should cooperatively abort any work you had in progress and release any resources your Worker is holding onto. 

#### onStopped callback
WorkManager invokes ListenableWorker.onStopped() as soon as your Worker has been stopped. Override this method to close any resources you may be holding onto. 

you can call isStopped method to chec if your worker has already been stopped if you're preforming long-running or repetivitve operation in your Worker, you should check this proeprty frequently and use it as a signal for stopping work as soon as possible

WorkManager ignores the Result set by a Worker that has received the onStop signal, because the Worker is already considered stopped. 
