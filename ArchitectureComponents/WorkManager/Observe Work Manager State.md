The following will observe what state the WorkTag is in. 

```
WorkManager.getInstance(requireContext).getWorkInfosByTagLiveData("MyWorkerTag")
  .observe(viewLifecycleOwner, Observer<List<WorkInfo>> { listWorkInfo -> 
    if(listWorkInfo.isNullOrEmpty()) return@Observer
    
    val workInfo = listWorkInfo[0]
    when(workInfo.state) {
      WorkInfo.State.Running -> {
        // Work is running
      }
      WorkInfo.State.CANCELLED ->  {
        // Work is cancelled
      }
      WorkInfo.State.SUCCEEDED -> {
        // Work has finished succesful.
      }
      
    }
  
  })
  ```
