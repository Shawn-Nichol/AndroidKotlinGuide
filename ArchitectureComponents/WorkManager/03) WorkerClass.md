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
