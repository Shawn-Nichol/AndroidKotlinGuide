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


## Observing Intermediate Worker Progress
WorkManager adds  first-class support for setting and observing intermediate progress for workers. If the worker was running while the app was in the foreground, this information can also be shown to the user using APIs which reutnr the LiveData of WorkInfoListenableWorker now supports the setProgressAsync() ApI, which allows it to persist intermediate progress. These API allow developers to set intermediate progress that can be observed by the UI. Progress is represented by the Data, type, which is a serializable contianer of properties(similar to input and output, and subject to the same restrictions).

Progress information can only be observed and updated while the ListenableWorker is running. Attempts to set progress on a ListenablWorker after it has completed its execution are ignored. You can also observe progress information by using the one of the getWorkInfoBy...() or getWorkInfoBy...LiveData() methods. These methods reutnr instances of WorkInfo, which has a new getProgress method that returns Data. 


You can use the CoroutineWorker objects setProgress() extension function to update progress information. 
```
class ProgressWorker(context: Context, param: WorkParameters) CoroutineWorker(context, param) {
    companion object {
        const val Progress = "Progress"
        private const val delayDuration = 1L
    }
    
    override fun doWork(): Result {
        val firstUpdate = workDataOf(Progress to 0)
        val lastUpdate = WorkDataOf(Progress to 100)
        
        setProgress(firstUpdate)
        delay(delayDuration)
        setProgress(lastUpdate)
        return Result.success()
    }
}
```

## Observing Progress
Observing progress information is also simple. You can use the getWorkInfoBy...() or getWorkInfoBy...LiveData() methods, and get a reference to WorkInfo.

```
WorkManager.getInstance(applicationContext)
    // requestId is the WorkRequest id
    .getWorkInfoByIdLiveData(requestId)
    .observe(observer, Observer { workInfo: WorkInfo? -> 
        if(workInfo != null) {
            val progress = workInfo.progres
            val value = progress.getInt(Progress, 0)
            // Do something. 
        }
```












