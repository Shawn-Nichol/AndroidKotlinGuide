# Getting started

In order to start a WorkManager you will need to do the following

- Add: dependices: Add gradle dependencies
- Create WorkerClass: the work will be run in the doWork() method
- Create WorkRequest: handles information about the Work and what type of WorkRequest. 
  - Constrataints: work is deffered until conditions are met. 
  - Data: information you can pass onto the worker. 
  - Tag: Uniquie Identifier
  - Retry and BackOff: parameters
- WorkManager: Passes the WorkRequest off to the JobScheduler.  
  - UniqueWork: guarntess there is only one instance of work with a particular name.
    - ConflicResolution: Tell WorkManager what todo if there is a conflict
  - ChainingWork: Links work together. 
    



## Add dependices
```
  def work_version = "2.4.0"
    // Kotlin + coroutines
    implementation "androidx.work:work-runtime-ktx:$work_version"
```

