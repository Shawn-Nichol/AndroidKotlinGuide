# What is a WorkManager

WorkManager is an API that makes it easy to schedule deferrable, asynchronous tasks that are expected to run even if the app exits or the device restarts. The WorkManager API is a suitable and recommeneded replacement for all previous Android background scheduling APIs. WorkManager incoropartes the features of its predecessors in a modern, consistent API that works back to API level 14 while also being conscious of batter life. 


## Key Features
### Work Constraints
Declaratively define the optimal conditionns for your work to run using Work Constraints. For example only run when device is connected to WiFi. 

### Robust scheduling
Sometimes work fails. WorkManaer offers flexible retry policies, including a configuragble exponential backoff policy

### Work Chaining
For complex related work, chain individual work tasks together, with an interface that allows you to control which pieces run sequentially and which run in parallel. For each work chain you can define input and output data. 

### Built-Int Threading Interoperability
WorkManager integrates seamlessly with RxJava and Coroutines and provides the flexibility to plug in your own asynchronous APIs

### Defferable and Reliable
WorkManger is intended for work that is deferrable(not required to run immediately) and rquired to run reliably even if the app exits or the device restarts

WorkManger is not intented for in-process background work that can safely be terminated if the app process goes away or for work that requires immediate execution.
