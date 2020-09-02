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


## Chaining  Work
WorkManager allows you to create and enqueu a chain of work that specifies multiple dependent tasks, and defines what order they should run in. This is particularly useful when you need to run several tasks in a particular order

To create a chain of work, use WorkManager.beginWith(OneTimeWorkRequest) or WorkManager.beginWith(List<OneTimeWorkREquest>), which return an instance of WorkContinuation.
  
A WorkContinuation can then be used to add dependent OnTimeWorkRequests using WorkContinuation.then(OneTimeWorkREqueset or
WorkContinuation.then(List<OneTimeWorkREquest>).
  
Every invocation of the WorkContainuation.then(...), reutnrs a new instance of WorkContinuation. If you add a List of OneTimeWorkREquets, these requests can potentially run in parallel. 
```
WorkManager.getInstance(myContext)
  // Candidates ro run in parallel
  .beginWith(listOf(filter1, filter2, filter3))
  // Dependent work(only runs after all previous work in chain .
  .then(compress)
  .then(upload)
  .enqueue()

```

## Input Mergers
When using chains of OneTimeWorkRequests, the output of parent OnteTimeWorkREquests are passed in as inputs to the children. So in teh above example, the outputs of filter1, filter2 and filter3 would be passed in as inputs to the compress request.

In order to manage inputs from multiple parent OneTimeWorkRequests, WorkManager uses InputMergers. There are two types of inputMergers provided by WorkManager
- OverwritingInputMerger attemtps to add all keys from all inputs to the oupt. In case of conflicts, it overwrites the previously-set keys. 
- ArrayCreatingInputMerger attempts to merge the inputs, creating arrays when necessary.

```
val compress: OneTimeWorkRequest = OneTimeWorkRequestBuilder<CompressWork>()
  .setInputMerger(ArrayCreatingInputMerger::class)
  .setConstratins(constraints)
  .build()
```

## Chaining and Work Status
There are a couple of things to keep in mind when creating chains of OneTimeWorkRequests
- Dependent OneTimeWorkRequests are only unblocked when all its parent OneTimeWorkRequests are successfull. 
- When any parent OneTimeWorkRequest fails, then all dependent OneTimeWorkRequests also fail
- When any parent OneTimeWorkRequest is cancelled, all dependent OneTimeWorkRequests are also marked as CANCELLED.