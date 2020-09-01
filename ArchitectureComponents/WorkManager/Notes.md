
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

# Work States
Work goes through a series of state changes over its lifetime. 

## One-time work states
For a one time work request, you work begins in an ENQUEUED state.

In the ENQUEUED state, your work is eligible to run as soon as its Constraints and initoa; delay timging requirements are met. From there it moves to a RUNNING state and then depending on theoutcome of the work it may move to SUCCEEDED, FAILED or possibly back to ENQUEUED if the resul tis retry. At any point in the process, work can be cancelled, at which point it will move to the CANCELLED state. SUCCEEDED, FAILED and CANCELLED all represent a terminal state of this owrk. If your work is in any of these states, WorkInfo.State.isFinished() returns true

## Periodic work states
Success and failed states apply only to one-time and chained work. For periodic work, there is only one terminal state, CANCELLED. This is becuase periodic work never ends. After each run, it's recheduled, regardless of the result. Figure 2 depicts the condensed state diagram for periodic work. 

## Block State
This state applies to work that is orchestrated in a series, or chain of work. Work chains, and their state diagram.


#Managing Work
Once you've defined your Worker and your WorkRequest, the last step is to enqueue your work. The simplest wway to enqueue work is to call the WorkManager enqueue() methods, passing the WorkRequest you want to run. 

```
val myWork: WorRequest = // ... OneTime or PeriodicWork
WorkManager.getInstance(requireContext()).enqueue(myWork)
```
Use caution when enqueuing work to avoid duplication. For example, an app might try to upload its logs to a backend service every 24 hours. If you aren't careful, you might end up enqueueing the same task many times, even through the hob only needs to run once. To achieve this goal, you can schedule the owrk as unique work. 

## Unique Work
Unique owrk is a powerful concept that guarantees that you only have one instance of work with a particular name at a time. Unlike IDs, unique names are human-readable and specified by the developer instead of being auto-generated by WorkManager. Unlike tags, unique names are only associated with a single instance of work. 

Unique work can be applied to both one-time and periodic work. You can createa unique work sequence by calling one of these methods, depending on whether you're scheduling repeating work or one time work. 
- WorkManager.enqueueUniqueWork: one-time
- WorkManager.enqueueUniquePEriodicWork(): Periodic work

Both of these methods accept 3 arguments
- uniqueWorkName: A string used to uniquely identify the work request.
- existingWorkPolicy: An enum which tells WorkManager what to do if there's already an unfinished chain of work with that unique name
- work: the WorkRequest to schedule
```
val sendLogsWorkRequest = 
  PeriodicWorkREquestBuilder<SendLogsWorker>(24, TimeUnit.HOURS)
    .setconstraints(Constraints.Builder()
      .setRequiresCharging(true)
      .build()
    )
    .build()
WorkManager.getInstance(this).enqueueUniquePeriodicWork(
  "sendlogs",
  ExistingPeriodicWOrkPolicy.KEEP, 
  sendLogsWorkRequest
)
```
Now, if the code runs while a sendLogs job is already in queue, the existing job is kept and no new job is added. Unique work sequences can also be useful if you need to gradually build up a long chain of tasks. For example, a photo editing app might let users undo a long chain of actions. Each of those undo operations might take a while, but they have to be performed in the correct order. In this case, the app could create an "undo" chain and append each undo operation to the chain as needed. See Chaining work for more details. 

## Conflict resolution policy
When scheduling unique work, you must tell WorkManager what action to take when there is a conflict. You do this by passing an enum when enquing the work. For one-time work you provide and ExistingWorkPolicy, which supports 4 options for handling the conflict
- Replace: existing work with the new work. This option cancels the existing work.
- Keep: existing work and ignore the new work. 
- Append: The new work to the end of the existing work. This policy will cuase your new work to be chained to the existing owrk, running after the existing work finishes. 

The existing work becomes a prerequisite to the new work. If the existing work becomes CANCELLED or FAILED, the new work is also CANCELLED or FAILED. If you want the new owrk to run regardless of the status of the existing work, use APPEND_OR_REPLACE instead. 

- APPEND_OR_REPLACE: functions imilarly to APPEND, except that it is not dependent on prerequisite work status. If the existing work is CANCELLED or FAILED, the new work stills runs. 

For period work, you provide an ExistingPeriodicWorkPolicy, which supports 2 options, REPLACE and KEEP. These options functionthe same as their Existing WorkPolicy counterparts

## Observing your work
At any point after enqueuing work, you can check its status by querying WorkManager by its name, id or by a tag associated with it. 

```
// by id
workManager.getWorkInfoById(SyncWorker.id)

// by name 
workManager.getWorkInfosForUniqueWork("sync")

// by tag
workManager.getWorkInfoByTag("synctag")
```

The Query returns a ListenableFuture of a WorkInfo object, which includes the id of the work, its tags, its current State, and any output data set via Result.success(outputData).

A LiveData variant of each of the methods allows you to observe changes to the WorkInfo by registering a listener. For example if you wanted to display a message to the user when some work finishes successfully, you could set it up as follows
```
workManager.getWorkInfoByIdLiveData(syncWorker.id)
  .observe(viewLifecycleOwner) { workInfo ->
    if(workInfo?.state == WorkInfo.State.SUCCEEDED) {
      Snackbar.make(requireView(),
      R.string.work_completed, Snackbar.LENGTH_SHORT)
        .show()
      }
    }
```
## Complext work Querries
WorkManager 2.4.0 and higher supports complex quering for enqueued jobs using WorkQuery objects. WorkQuery supports querying for work by a combination of it's tags, state and unique work name

How find work with a tag
```
val workQuery = WorkQuery.Builder
  .fromTags(listOf("syntTag")
  .addStates(listOf(WorkInfo.State.FAILED, WorkInfo.State.CANCELLED))
  .addUniqueWorkNames(listOf("preProcess", "sync")
  ).build()
val workInfos: ListenableFuture<List<WorkInfo>> = workManager.getWorkInfos(workQuery)

```
Each component(tag, state, or name) in a WorkQuery is AND with the others. Each value in a component is OR. 

## Cancelling and stopping work
If you no longer need your previously enqueued work to run, you can ask for it to be cancelled. Work can be cancelled by its name, id or by a tag associated with it. 

```
// by id
workManager.canccelWorkById(syncWorker.id)

// By name
workManager.cancelUniqueWork("sync")

// By tag
workManager.cancelAllWorkByTag("syncTag")
```
Under the hood, WorkManager checks the State of the work. If the work is already finished, nothing happens. Otherwise the work's state is chagned to CANCELLED and the work will not run in the future. Any WorkRequest jobs that are dpendent on this work willl also be CANCELLED. Currently RUNNING work receivves a clal to ListenableWorker. onStopped(). Override this method to handle any potenial cleanup

## Stop a running Worker
There a a few differen reasons your runing Worker might be stopped by WorkManager:
- You explicitly asked for it be cancelled: WorkManager.cancelWorkById(UUID)
- In the case of unique work, you explicitly enqueed a new WorkRequest with an Existing WorkPolicy of Replac.e The old WorkRequest is immediately considered cancelled. 
- Youur work's constraints are no longer met
- The sytem instucted your app to stop your work for some reason. This can happend if you eceed the execution deadline of 10 minutes. The work is scheduled for retry at a later time. 

Under these conditions, your worker is stopped

You should cooperatively abort any work you had in progresss and release any resources your worker is holding onot. For example, you shoul dcolose open handles to databases and files at this point. There are two mechanisms at your diposal to understand your worker is stopping

### onStopped() Callback
WorkManager invokes ListenableWorker.onStopped() as soon as your Worker has been stopped. OVerride this method to close any resources you may be holding onto. 

### isStopped property
You can call the ListenableWOrker.isStopped() method to check if your worker has already been stopped. Ifyou're performing long-running or repetivite operations in your Worker, you should check this property frequently and use it as a signal for stopping work as soon as possible. 

Note WorkManager ignores the Result set by a Worker that has received the onStop signal, becuase the Worker is already considered stopped. 

# Observing intermediate Worker progress
WorkManager adds first-class support for setting an d observing intermediate progress for workers. If the worker was running while the app was in the foreground, this information can also be shown to the useer using APIs which return the LiveData of WorkInfo.

ListeneableWorker now supports the setProgessAsync() API, which allows it to persists intermediate progress. These APIs allow developers to set intermediate prgoress that can be observed by the UI. Progress is represented by the Data type, which is a serializable container of properties. (similar to input and output and subject to the same restrictions).

Progress inofrmation can only be observed and updated while the ListenableWorrker is running. Attempts to set progress on a ListenableWorker after it has completed its execution are ignored. You can also observe progress inofrmation by using one of the getWorkInofBy...() or getWorkInfoBy..LiveData methods. These methods reutrn instances of WorkInfo, which has a new getProgress() method that returns data. 

## Updating Progress
Use the Coroutineworker object's setProgress extension function to update progress to 0 when it starts an upon completion updates the progress value to 100. 
```
class ProgressWoker(context: Context, parameters: WorkParameters) : 
  CoroutineWoker(context, parameters) {
 
  companion object {
    const val Progress = "Progress"
    private const val delay Duration = 1L
  }
  
  override suspend fun doWork() : Result {
    val firstUpdate = workDataOf(Progress to 0)
    val lastUpdate = workDataOf(Progress to 100)
    setProgress(firstUPdate)
    delay(delayDuration)
    setProgress(lastUpdate)
    return Result.success()
  }
}
```

## Observing Progress
You can use the getworkInfoBy...() or getWorkInfoBy...LiveData() methods, and get  reference to WorkInfo.
Here is an example which uses the getWorkInfoByIdLiveData API
```
WorkManager.getInstance(applicationContext)
  // request is teh WorkRequest id
  .getWorkInfoByIdLiveData(requestId)
  .observe(observer, Observer { workInfo: WorkInfo? -> 
    if(workInfo != null) {
      val progress = workInfo.progress
      val value = progress.getInt(Progress, 0)
      // Do something with progress information
    }
  })
```

# Chaining Work 
WorkManager allows you to create and enqueue a chain of work that specifies multiple dependent tasks, and defines what order they should run in . This is particularly useful when you need to run several tasks in a particular order

To createa chain of work, you can use WorkManager.beginWith(OneTimeWorkRequest) or WorkManager.beginWith(List<OneTimeWorkREquest>), which returns an instance of WorkContinuation.
  
A WorkContinuation can then be used to add dependent OneTimeWorkREquets using WorkContinuation.Then(OneTimeWorkREquest) or WorkContinuation.Then(Lost<OneTimeWorkREquest>).
  
Every invocation of the WorkContinuation.then(...) returns a new instance of WorkContinuation. If you add a list of OneTimWorkRequests, these requests can potentially WorkContinuation. If you add a List of OneTimeWorkREquets, these requests can potenially run in parallel. 
  
Finally, you can use the WorkContinuation.enqueue() method to enqueue() yoiur chain of WorkContinuations
  
Lets look at an example where an applicatioon runs image filters on 3 differen images (potentially in parallel), then compresses those images together, and then uploads them.
```
WorkManger.getInstance(myContext)
  // Candidates ro run in parallel
  .beginWith(listOf(filter1, filter2, filter3))
  // Dependent work (only runs after all previous work in chain)
  .then(compress)
  .then(upload)
  .enqueue()
```

## Input Mergers
When using chains of OneTimeWorkRequest the output of parent OneTimeWorkRequests are passed in as inputs to the children. So in the above example, the output of filter1, filter2 an filter 3 would be passed in as inputs to the compress request. 

In order to manage inputs from multiple parent OneTimeWorkREquets, WorkManager uses InputMergers

There are two different types of Inputmergers provided by WorkManager
- OverwritingInputMerger attempts to add all keys from all input to the output. In case of conflicts, it overwrites theh previously-set keys. 
- ArrayCreatingInputMerger attempts to merge the inputs, creating arrays when necessary

For teh above example, given we want to preserve the outputs from all image filters, we should use an ArrayCreatingInputMerger. 
```
val compress: OntTimeWorkRequest = OneTimeWorkRequestBuilder<CompressWorker>()
  .setInputMerger(ArrayCreatingInputMerger::class)
  .setConstraintst(onconstraints)
  .build()
```

## Chaining and Work Statuses
There are a couple of things to keep in mind when creating chains of OneTimeWorkREquets
- Dependent OneTimeWorkREquests are only unblocked(transition to ENQUEUED), when all its parent OneTimeWorkRequests are successsful(that is, they return a Result.success()().
- When any parent OneTimeWorkREquest fails(returns a Result.failure(), then all dependent OneTimeWorkREquest are also marked as FAILED. 
- When any parent OneTimeWorkREquest is cancelled, all dependent OneTimeWorkRequests are also marked as CANCELLED. 


