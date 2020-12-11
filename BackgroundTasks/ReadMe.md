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
