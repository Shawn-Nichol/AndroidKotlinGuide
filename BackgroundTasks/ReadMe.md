# Threads
All  adnroid apps use a main thread to and thandel UI oeprations. calling long-running oeprations from the main thread can lead to freezes and unresponsive apps. For example apps that makes network request from the main thread wil freeze your UI on the app receives a response a network response. Create additional threads to handle long running operations while the main thread continues to handle UI updates. 

# Background Tasks
Any task that takes more than a few milliseconds should be delegated to a background thread. Common long-running tasks include things like decoding a bitmap, acccessing storage, working on a mahicne learning model, or perform network requersts. 

An app is running in the background when
1. None of the app's activites are currently visible to the user.
2. The app isnt' running any foreground services that started while an activity from the app was visible to the user. 

The following list shows common pending tasks that an app manages while it runs in the background
- Your app registers a broadcast receiver in the manifest
- Your app schedules a repeating alarm Using Alarm Manager
- Your app shcedules a background task, either as a worker using WorkManager or a job using Job scheduler. 

Background tasks fall into one of the following categories

## Immediate 
Tasks that need to be completed while the user is interacting with application.

- Use kotlin coroutines for tasks that should end when the user leaves a certain scope of finsishes an interaction. Many Android KTX libraries contain ready to use coroutines scopes for common app compoents  like ViewModel and common application lifecycles. 

- For tasks that should be executed immdiately and need continued processing, even if the user puts the applicatoin  in background or the device restarts, we recommendd uing WorkManager and its support for long-running tasks. 

## Deferred 
Tasks that don't need to run at an exact time.

- Every taks that is not directly connected to the a user interaction and can run at any time in the furture can be deferred. The recommended solution for deffered tasks is WorkManager. WorkManager makes it easy to schedule defferable, ashynchrounou stasks that are expected to run even if the app exits or the device restarts. 

## Exact
Tasks that need to run at an exact time. 
To categorize a task, answer the following questions, and traverse the corresponndign decision tree

- A  task that needs to be execute at an exact point in time can use AlarmManager.



# Glossary
## JobScheduler
This is an API for scheduling various types of jobs against the framework that will be executed in the application own process. `JobInfo` objects will be passed to `JobScheduler` with `schedule()`. When the criteria declared are met, the system will execute this job on your application `JobService`. Identify  the service component that implements the logic for your job when you construct the JobInfo.

The framework will be intelligent about when it executes jobs, and attempt to batch and deffer them as much as possible. Typically if you don't specify a deadline on a job, it can be run at any moment depending on the current state of the JobScheduler's internal  queue. 

## JobInfo
Container of data passed to the `JobScheduler` fully encapsulating the parameters required to schedule work against the calling application. These are constructed using the `JobInfo.Builder`. The goal here is to provide the scheduler with high-level semantics about thw rwork you want to accomplish

## JobService
Entry point for the callback from the `JobScheduler`. This is the base class that handles asynchonous requests that were previously scheduled. you are responsible for overriding `JobService#onStartJob(JobParameters)`, which is where you will implement your job logic

This service executes each incomign job on  a Handler running  on your applications main thread. This means that you must offload your executio nlogic to another thread/handler/Asynctask of your choosing. not doing so will result in flocking any future callbacks from the JobManager specificially `onStop()`, which is meant to inform you that the scheuldling requirements are no longer being met. 


## WorkManger 
`WorkManager` API makes it easy to schedule deferrable, asynchronous tasks that must be run reliably. These APIs let you createa task and hand it off to WorkManager to Run when the work contraints are met. 

## Executor 
An object that executes submitted Runnable tasks. This interface provides a way of decoupling tasks submission from the mechanics of how each task will be run, including details of thread use., scheduling etc. An Executor is normally used instead of explicitly creating threads
```
Executor excutor = anExecutor()
executor.execute(new RunnableTask1())
executor.execute(new RunnableTask2())
```


## Executor service
An executor that provides methods to manage termination and methods that can produce a Future for tracking progress of one or more asynchronous tasks. An ExecutorSErvice can be shutdown, whcih will cause it to reject new tasks. Two different methods are provided for shutting down and ExecutorService. The shudown() method will allow previously submitted tasks to execute before terminating, while the shutdownNow() method prevents wating tasks from starting and attempts to stop currently executing tasks. upong termination an executor has not tasks atviely executing, nono tasks awating execution, and no new tasks can be submitted. An unused ExecutorService should be sut down to allow reclamation of its resrouces. Method sumbit extenese base method Exec

## Future
A future represents the result of an asynchronous computation. Methods are  provided to check if the computations  is complete, to wait for its completion, and to retrieve the result of teh computation. The result can only be retrieved using method get when the computation has completed, blocking if necessary until it is ready. Cancellation is perfromed by the canel method. Additional methods are provided to determine if the task completed normally or was cancelled. Once a computation has completed, the computation cannot be cancelled. If you would like to use a future for the sake of cancellability but not provide a usable result, you can declare types of the from future and return null as a result of the underlying tasks. 

## ThreadPoolExecutor
An ExecutorSErvice that executes each submitted task using one of possibly several pooled threads, normally configured using executors factory methdos. Thread pools address two different problems: they usually provide improved perfromance when executing large numbers of asynchronous tasks, due to reduced per-task invocation overhead, and theyprovide a means of bounding and managing the resources, inlcuding threads, consumed when executing a collection of tasks. Each ThreadPoolExecutor also maintains some basic statistics such as the number of completed tasks. 


## immutable data
Immutable can be used to mark class as producing immutable instances. The immutability of the class is not validated and is a promise by the tyhpe that ll publicly accessible properties and fields will not change after the instance is constructed. This is a stronger promise than val as it promises that the value will enver change not only that values cannot be changed through a setter. 

Immutable is used by composition which enables composition optimizations that can be perfromed based on the assumption that values read from th etype will not change. 

Data classes that only contain val properties that do not have custom getters can safely be marked as Immutable if thje types of preoprties are either primitive types or also Immutable. 

## Threadsafe
In some situations, the methods you implement might be called from more than one thread, and therefore  must be written to be trehad-safe.

The primarily  tture for methods that can be called remotely such as methods in a bound service. When a call on a method implemnted in aa IBinder origniates inthe same process in which the IBinder is running, the methods is executed in the callers thread. However when the call orignates in antoehr process, the methods is executed in a thread chosen from a ppol of threads that the system maintains in the same processas as the IBinder it's not executed in the UI thread or the process. For example wheras a servies onBind method would be called form the UI  thread of the service process, methods implemented int he object that onBind returns for exmaplle a subclass that implemtns pool thread can engage the same IBinder method at the same time

## Handlers
A Handler allows you to send and process Message and runable objectss associated with a thread's Message quueue Each handler instance is assocated with a single thread and that thread's message queue. When you create a new Handler it is bound to a looper. It will deliver messages and runnables to that Loopers message queue ane executge them on the Loopers thread. 
1. To schedule messages and runnables to be executed at some point in the future
2. To enqueue an action to be performed on a different thread than your own. 

## Looper
Class used to run  mesage loop for a  thread. Threads by default do not have a message loop associated with them too create one, call prepare() in the thread that is to run the loop, and then loop9) to have it process messages until the loop is stopped. Most interaction with a message looop is throgh the handler class. 
