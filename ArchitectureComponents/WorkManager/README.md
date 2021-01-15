# Work Manager
WorkManager is an API that makes it easy to schedule defferable, asynchronous tasks thata are expected to run even if the app exits or the device restarts. The WokrMnaager API is suitable and recommended replacement for all previous Android background scheduling APIs,. 





## Define WorkRequest
Set the the details of the workRequest

`Schedule`
There are two types of schedules, 
- OneTimeWorkRequest: only scheduled once.
- PeriodicWorkRequest: schedules work that repeats on some interval

`Work Constraints `
Define the optimal conditions for the work to run with Work Constraints. 
- NetworkType: Constrains the type of network requried for yoru work to run. 
- BatteryNotLow: Work will not run if the device is in ow battery
- RequiresCharging: Work will only run when the device is charging
- DeviceIdle: This requires the user's device to be idle before th work will run. This can be useful for running batched operations that might otherwise have a negative perfromance impact on other apps running actively on the user's device. 
- StorageNotLow: Work will not run it the user's storage space on the device is too low. 

`DelayWork` 
In the event that your work has no constraints or that all the constraints are met when your work is enqueued, the system may choose to run the work immediately. If you do not want to work to be run immediately, you can specify your work to start arfter a mimum initial delay. 

`BackOff Policy` 
If you need to tetry your work, return Result.retry()

- Backoff delay specifies the mimum amount of time to wwait before retrying your work after the first attempt. THis value can be no less than 10 seconds.
- Backoff policy defines how the backoff delay should increase over time for subsequent retry attempts. WorkManager supports 2 backoff polcies,
  - LINEAR: WorkManager will increase the Backoff time linearly
  - EXPONENTIAL: WorkManager should increase the backoff time exponentially.

`Tag Work`
Work request have a unique identifier, which can be used to identify that work later in order to cancel the work or observe its progress.

`Assign inputData` 
Is a Data object used to passinfo to doWork()



## Worker class
Work is defined using the Worker class. The doWork() methdos runs asyncrhonously on a background trhead provided by WorkManager. 

`Results` 
The result returned  from doWork() informs the WorkManager service whether the work seucceeded, and in the case of failure whether or not the work needs to be retried. 
- Result.success(): The work finished successfully.
- The Result.failure(): The work failed
- Result.retry(): The work failed and should be tried at another time according to its retry policy. 

`enque` 
Submits the WorkRequest to the WorkManager.
Resulotion policys
- REPLACE: existing work with the new work. This options cancels the existing work
- KEEP: existing work and ignore the new work
- APPEND: the new work to the end of the existing work. This policy will cause your new work to be cachained to the existing work, running after the existing work finishes. 
- APPEND or REPLACE: functions similarly to APPEND, except that it is not dependent on prerequisite work status. If the existing work is CANCELLED or FAILED, the new work still runs. 


`Cancel and Stopping Work`
If the work no longer needs to run you can request it to be canceled. 
