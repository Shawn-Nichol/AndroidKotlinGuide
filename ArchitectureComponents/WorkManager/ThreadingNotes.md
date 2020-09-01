# Threading in Worker
When you use a Worker, WorkManager automatically calls Worker.doWork() on a background thread. The background thread comes from the executor specified in WorkManager's Configuration. By defualt, WorkManager sets up an Executor for you but you can also customize your own. For example, you can share an existing background Executor in your app, or create a single-threaded Executor to make sure all your background work executes serially, or even specify a ThreadPool with a different thread count. To custtomize the Executor, make sure you have enabled manual initialization of WorkManager. Whne configuring WorkManager, you can specify your Executor as.

```
WorkManager.initialize(
  context, COnfiguration.Builder()
    .setExecutor(Executors.newFixedThreadPool(8))
    .build())
```

Worker
```
class DownloadWorker(context: Context, params: WorkParameters) : Worker(context, params) {
  override fun doWork(): ListenableWorker.Result {
    for (i in 0..99) {
      try {
        downloadSynchronoulsy("www.tsn.ca")
      } catch (e: IOException) {
        return ListenableWorker.REsult.failure()
      }
    }
    return ListenableWorker.Result.successs()
  }
}

```

Note that Worker.doWork() is synchronous call - you are expected to do the entiery of your background work in a blocking fashion and finish it by the time the method exits. If you all an asynchronous API in doWork() and return a Result, your callback may not operate properly. If you asynchronous API in doWork() and reutn a result, your callback may not operate properly. If you find yourself in this situation, consider using a ListenableWorker

When a currently running Worker is topped for any reason, it receives a call to Worker.onStopped(). Override this method or call Worker.isStopped() to checkpoint your code and fee up resources when necessary. When the worker in the example above is stopped, it may be in the middle of its loop of downloading items and will continue doing so even though it has been stopped. To optimize this behavior, you can do something like this.

```
class DownloadWorker(context: Context, params: WorkerParamters) : Worker(context, params) {
  override fun doWork(): ListenableWorker.Result {
    for(i in 0..99) {
      if(isStopped) break
      
      try {
        downloaodSynchronously("www.tsn.ca")
      } catch (e: IOException) {
        return ListenableWorker.Result.failure()
      }
    }
    
    return listenableWorker.Result.success()
  }
}

```

Once a worker is stopped it doesn't matter what you return from do work

# Threading in CoroutineWorker
WorkManager provides first-class support for coroutines. To get started, includ work-runtime-ktx in your gradlefile. Instead of extedning Worker, you should extend CoroutineWorker, which has a slightly differen API. For example, if you wanted to build a simple CoroutineWorker to perform some network operationns, you would do the following. 

```
class CoroutineDownloadWorker(context: Context, params: WorkerParameters) : CoroutineWorker(context, params)

  override suspend fun doWork(): Result = coroutineScope {
    val jobs = (0 until 100).map {
    async {
      downloadSynchronouslly("www.tsn.ca")
    }
  }
  
  // awaitAll will throw an exception if a download fails, which CoroutineWorker
  jobs.awaitAll()
  Result.success()
```

Note that CoroutineWorker.doWork() is a suspending function. Unlike Worker, this code does nnot run on the Executor specified in your Configuration. Instead, it default to Dispatchers.Default. You can customize this by provding your own Coroutinecontext. In the above example, you would probably wnat to do this work on Dispatchers.IO 
```
class CoroutineDownloadWorker(context: Context, params: WorkerParameters) : CoroutineWorker(context, params) {
  override val coroutineContext = Dispatchers.IO
  
  override suspend fun doWork(): Result = coroutineScope {
    val jobs = (0 until 100).map {
      async {
        downloadSynchronously("www.tsn.ca")
      }
    }
    // await all will throw an execption if a download fails, which Coroutine works
    jobs.awaitAll()
    Result.success()
  }
}
```

CoroutineWorkers handle stopppage automatically by cancelling the coroutine and propagating the cancellation signals. You don't need to do anything special to handle work stoppages. 
