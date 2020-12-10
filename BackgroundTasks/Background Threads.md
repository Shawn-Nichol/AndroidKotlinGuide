# Running Anroid tasks in background threads
All android apps use a main thread to handle UI operations. Calling long-running operations from the main thread can lead to freezes and unrespponsiveness. For example, if your app makes a network request form the main thread, your app's UI is frozen until it receives the network response. You can create additional background threads to handle along-running operations while the main thread continues to handle UI updates. 

## Thread Pool, Creating Multiple threads
A thread pool is a managed collection of threads that runs tasks in parallel from a queue. New task are executed on existing thread as those threads become idle. To send a task to a thread pool, use the executor service interface. Note that executor service has nothing to do with services. 

Creating threads is expensive, only create threads during app intitialization. Be sure to save the instance of the Executorservice either in your application class or in a dependency injection container. 

 
Thread pool that creates 4 threads.
```
class MyApplication : Application() {
  val executorService: ExecutorSErvice = Executors.newFixedThreadPool(4)
}
```

## Executing on a backgorund thread
Making a network request on the main thread blocks the main thread until it receives a response. The OS can't call onDraw(), and the app freezes, potenially leading to an Applcaitoin Not Responding (ANR). 

How to run on a backbround thread.
```
sealed class Result<out R> {
    data class Success<out T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
}

class LoginRepository(private val responseParser: LoginResponseParser) {
    private const val loginUrl = "https://example.com/login"

    // Function that makes the network request, blocking the current thread
    fun makeLoginRequest(
        jsonBody: String
    ): Result<LoginResponse> {
        val url = URL(loginUrl)
        (url.openConnection() as? HttpURLConnection)?.run {
            requestMethod = "POST"
            setRequestProperty("Content-Type", "application/json; charset=utf-8")
            setRequestProperty("Accept", "application/json")
            doOutput = true
            outputStream.write(jsonBody.toByteArray())

            return Result.Success(responseParser.parse(inputStream))
        }
        return Result.Error(Exception("Cannot open HttpURLConnection"))
    }
}
```

makeLoginRequest() is synchronous and blocks the calling thead. To model the response of the network request, we have our own result class. 
The ViewModel triggers the network request when the user taps on a button. 
```
class LoginViewModel(
    private val loginRepository: LoginRepository
) {
    fun makeLoginRequest(username: String, token: String) {
        val jsonBody = "{ username: \"$username\", token: \"$token\"}"
        loginRepository.makeLoginRequest(jsonBody)
    }
}
```

With the previous code, LoginViewModel is blocking the main thread when making the network request. We can use th thread pool that we've instantiated to move the execution to a backgound thread. First following the principles of dependcy injection, loginRepository takes an instance of executor as opposed to executore service because it's executing code and not managingf threads. 

```
class LoginRepository(
    private val responseParser: LoginResponseParser
    private val executor: Executor
) { ... }
```

The executor's execute() method takes a runnable. A runnable is a single abstarct method SAM interface with a run method that is executed in a thread when invoked

Let's creat another function called makeLoginRequest that moves the execution to the backgroun thread and ignros the response for now. 

```
class LoginRepository(
    private val responseParser: LoginResponseParser
    private val executor: Executor
) {

    fun makeLoginRequest(jsonBody: String) {
        executor.execute {
            val ignoredResponse = makeSynchronousLoginRequest(url, jsonBody)
        }
    }

    private fun makeSynchronousLoginRequest(
        jsonBody: String
    ): Result<LoginResponse> {
        ... // HttpURLConnection logic
    }
}
```

Inside the execute() method, we create a new Runnable with the block of code we want to execute in the background thread in our case, the synchronous network requeset method. Internally, the executor service manges the runable and exsecutes it in ana vailable thread. 























## Considerations
Any thread in your app can run in parallel to other threads, including the main thread, so you should ensure that your codei s thread-safe. Notice that in our example that we avoid writing to variables shared between threads, passing immutable data instead. This is a good practice, becuase each thread works with its own instance of data, and we avoid the complexity of synchronization. 

## Communciating with the main thread
In the previou step, we ignrored the network requeest response. To display the reusl on the screen, login viewMdoel needs to know about it. We can do that by using callbacks

The function make LoginRequest() should tak a callback as a prameter so that it can return a value asynchronously. the callback with the resul is called whenever th network request completes or a failure occurs. In kotlin, we can use a higher-order function. However, inn java we have to createa  new callback interface to have the same functionality

```
class LoginRepository(
    private val responseParser: LoginResponseParser
    private val executor: Executor
) {

    fun makeLoginRequest(
        jsonBody: String,
        callback: (Result<LoginResponse>) -> Unit
    ) {
        executor.execute {
            try {
                val response = makeSynchronousLoginRequest(jsonBody)
                callback(response)
            } catch (e: Exception) {
                val errorResult = Result.Error(e)
                callback(errorResult)
            }
        }
    }
    ...
}
```

The ViewModel needs to implement the callback now. It can perfrom different logic dpeneding on the result. 

```
class LoginViewModel(
    private val loginRepository: LoginRepository
) {
    fun makeLoginRequest(username: String, token: String) {
        val jsonBody = "{ username: \"$username\", token: \"$token\"}"
        loginRepository.makeLoginRequest(jsonBody) { result ->
            when(result) {
                is Result.Success<LoginResponse> -> // Happy path
                else -> // Show error in UI
            }
        }
    }
}
```
In this example the callback is executed in teh calling thread, whichis a backgroun thread. This means that you cannot modify or communicate directly with the UI layer until you switch back to the main thread. 

Using handlers 
You can use a handler to enqueue an action to be performed on a different thread. To specify the thread on which to run the aciton, construct the handler using a looper for the thread. A looper is an object that runs the message loop for an associated thread. Once you've created a handler, t you can then use th epost(ruunable) method to run a block of code in the corresponding thread. 

Looper includes a helper function getMainLooper(), which retrieves the looper of the main thread. you can run code in the main thread by using this looper to create a handler. As this is somethign you might do quite often, you can also save an instance of the hadnler int eh same place you saved the executorSErvice

```
class MyApplication : Application() {
    val executorService: ExecutorService = Executors.newFixedThreadPool(4)
    val mainThreadHandler: Handler = HandlerCompat.createAsync(Looper.getMainLooper())
}
```

It's good practtice ot inject the handler to the respository, as it gives you  more flexibility. For example, in the future you might want to pass in a different handler to sechule tasks on a separate thread. if you're always communcating back to the same thread, you ca pass the Handler into the Repository construcotor. 

```
class LoginRepository(
    ...
    private val resultHandler: Handler
) {

    fun makeLoginRequest(
        jsonBody: String,
        callback: (Result<LoginResponse>) -> Unit
    ) {
          executor.execute {
              try {
                  val response = makeSynchronousLoginRequest(jsonBody)
                  resultHandler.post { callback(response) }
              } catch (e: Exception) {
                  val errorResult = Result.Error(e)
                  resultHandler.post { callback(errorResult) }
              }
          }
    }
    ...
}
```

If you want more flexiblity you can pass in a Handler to each function

```
class LoginRepository(...) {
    ...
    fun makeLoginRequest(
        jsonBody: String,
        resultHandler: Handler,
        callback: (Result<LoginResponse>) -> Unit
    ) {
        executor.execute {
            try {
                val response = makeSynchronousLoginRequest(jsonBody)
                resultHandler.post { callback(response) }
            } catch (e: Exception) {
                val errorResult = Result.Error(e)
                resultHandler.post { callback(errorResult) }
            }
        }
    }
}
    ...
}
```
In this example, the callback passed into the Repository's makeLogin Request call is executed on teh main thread. That means you can directly modify the UI from the callaback or use LiveData.setValue to communciate with the UI. 

## Configuring a thread pool
You can create a thread pool using one of the executor helper functiosn with predefined settings, as shown i nthe previous example code. Alterntively, if you want to customize th details of the thread pool, you can create an instance using ThreadPoolExecutor directly. you can configure the following detials. 

- Initial and maximum pool size. 
- Keep alive time and time unit. Keep alive time is the maximum duration that a thread can remain idle beofre it shuts down. 
- An input queu that hold Runnable tasks. This queue must implement the Blocking Queue interface. To mtach the requirments of your app, you can choose from the available queue implementations to learn more, see the class overview for ThreadPopolExecutor. 
```
class MyApplication : Application() {
    /*
     * Gets the number of available cores
     * (not always the same as the maximum number of cores)
     */
    private val NUMBER_OF_CORES = Runtime.getRuntime().availableProcessors()

    // Instantiates the queue of Runnables as a LinkedBlockingQueue
    private val workQueue: BlockingQueue<Runnable> =
            LinkedBlockingQueue<Runnable>()

    // Sets the amount of time an idle thread waits before terminating
    private const val KEEP_ALIVE_TIME = 1L
    // Sets the Time Unit to seconds
    private val KEEP_ALIVE_TIME_UNIT = TimeUnit.SECONDS
    // Creates a thread pool manager
    private val threadPoolExecutor: ThreadPoolExecutor = ThreadPoolExecutor(
            NUMBER_OF_CORES,       // Initial pool size
            NUMBER_OF_CORES,       // Max pool size
            KEEP_ALIVE_TIME,
            KEEP_ALIVE_TIME_UNIT,
            workQueue
    )
}
```
## Concurrency libraries
It's important to understand the basics of threading and its underlying mechanisms. There are however, many popular libraries that offer higher-level abstraction over these concept and ready to use utilities for passing data between threads. These libraries include Guava and RxJava for the Java Programming language users and coroutines which we reommend for kotlin user
