# Observing Work
At any point after enqueuing work, you can check the status by quering WorkManager by its name, ID or by tag associated with the work.
```
// by id , ListenableFuture<WorkInfo>
WorkManager.getWorkInfoById(myWork.id

// By name, ListenableFture<List<WorkInfo>> 
workManager.getWorkInfoForUniqueWork("myWork")

// By Tag, ListenableFuture<List<WorkInfo>>
workManager.getWorkInfoByTag("myTag")
```

Query returns a ListenableFuture of WorkInfo object, which includes the ID of the work, its tags, its current state, and the output data set via Result.success(outputData). A LiveData vaiant of each of the methods allow syou to observe changes to the WorkInfo by registering a listener. 

```
workManager.getWorkInfoByIdLiveData(syncWorker.id)
  .observe(viewLifecycleOwner) { workInfo -> 
    if(workInfo?.state == WorkInfo.State.SUCCEEDED) {
      Snackbar.make(requireView(), 
        "Completed, Snackbar.LENGTH_SHORT
      ).show()
```

## Observing Progress
Observing progress information is also simple. You can use the getWorkInfo By or getWorkInoBy LiveData methods and geta  reference to WorkInfo.
```
WorkManager.getInstance(applicationContext)
  // requestId is the WorkRequest ID.
  .getWorkInfoByIdLiveDAta(requesteId)
  .observe(observer, Observer { workInfo ->
    if(workInfo != null) {
      val progress = workInfo.progress
      val value = progress.getInt(Progress, 0)
      // Do something. 
    }
  
```
