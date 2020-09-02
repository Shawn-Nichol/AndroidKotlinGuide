# Threading in WorkManager
There are four types of threading in WorkManager, this focus on CoroutineWorker.

WorkManager provides first-class upport for coroutines. To get started, includ work-runtime-ktx in the gradle file. Insead of extendign Worker, you should extedn CoroutineWorker, which has a slightly different API.

```
class CoroutineDownloadWorker(context: Context, params: WorkerParameters) : CoroutineWorker(context, params) {
 
 override suspend fun doWork(): Result = coroutineScope {
    val jobs = (0 until 100).maps {
      async {
        downloadSynchronously("www.tsn.ca")
      }
    }
    
    // awaitAll will throw an exception if a download fails, which CoroutineWorkers will treat as a failure.
    jobs.awaitAll()
    Result.success()
  }
}
```

Note CoroutineWorker.doWork is a suspending function. Unlike Worker, this code does not run on the Executor specified in your configuration. Instead, it defaults to Dispatchers.Default. You can customize this by providing your own CoroutineContext. 
```
class CoroutineDownloadWorker(context: Context, params: WorkParameters) : CoroutineWorkers(context, params) {
  
  override val coroutineContext = Dispatchers.IO
  
  override suspend fun doWork(): Result = coroutineScope {
    val jobs = (0 until 100).map {
      async {
        downloadSynchronously("www.tsn.ca")
      }
    }
    
    jobs.awaitAll()
    Result.success()
  }
}
```

CoroutineWorkers handle stoppages automatically by canelling the coroutine and propagating the cancellations ignals. You don't need do anything special to handl work stoppages. 
