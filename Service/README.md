A service is an application component that can perform long-running operations in the background, start a new thread to complete work. It does not provide a user interface. once started, a service might continue running for some time, even after the user switches to another application. 

A service is a component that can run in the background, even when the user is not interacting with the application, this is when you should create a service. Work performed ouside of the main thread well the app is in the foreground should use a another thread. 

## Types of Service
`Background`
A background service perfroms an operation that isn't visble to the user. 

`Foreground`
A foreground service perfroms some operations that is noticeablle t the user. A foreground service must display  a notification so that users are actively aware that he service is running. This notification cannot be dismissed unless the service is either stopped or removed from the foreground. 

`Bound`
A service is bound when an application component binds to it by aclling bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, receive results, and even do so across processes with interprocess communication (IPC). A bound service runs only as long as an application component is bound to it. Multiple componnents can bind to the service at once. 

## Started Service
A started service starts by calling startService, which calls onStartCommand(). 

`startService()`
Requests that a given application service be started. The intent should either contian the complete class name of a specific service implementation to start, or specific package name to target. `startService()` overrides the `bindService()` and remains running until `stopService()` is called. 

`startForegroundService()`
Similar to startService() but with an implicit promise that the service will call startForeground() once it begins running. The service is given an amount of time comparable to the ANR interval to do this, otherwise the system will automaticallly stop the service and declare the app ANR. 

## Stop Service
`stopService()`
Request that a given application service be stopped. If the service is not running, nothing happens. Otherwise it is stopped Note that calls to startService are not counted Ths stops the service no matter how many times it was started. 

`stopSelf()`
Stop the service, if it was previously started. This is the same as calling stopService().

## Service Class

`onStartCommand`
Called by the system every time a client explicitly starts the service by calling Context.startService() providing the arguments it supplied and a unique integer token representing the start request. 

return: The Return value indicates what semantics the system should use for the service's current started state.
  - START_NOT_STICKY: If the system kills the service after onStartCommand() returns, do not recrete the service unless there are pending intents to deliver.
  - START_STICKY: If the system kills the service after onStartCommand() returns, recreate the service and call onStartCommand(), do not redilver the last intent. Instead the system call `onStartCommand` with a null intent. 
  - START_REDELIVER_INTENT: If the system kills the service after `onStartCommand()` returns, recreate the service and call `onStartCommnad()` with the last intent that was delivered to the service. Any pending intents are delivered in turn. This is suitable for services that are actively perfroming a job that should be immediately resumed, such as downloading a file. 
  
  
  
`BindService `
Connection to an application service, creating it if needed. This defines a dependency between your application and the service. The given conn will receive the service object when it is created and be told if it dies and restarts. The service will be considered required by the system only for as long as the calling context exists. If the service doesn't require binding then it may return null. 

Parameters: 
service: Intent identifies the service to connect to. The intent must specify an explicit component name. 
conn: ServiceConnection: receives  information as the service is started and stopped. This must be a valid ServiceConnection object, 
flags: Operation options for the binding. 
return  Boolean: True if the system is in the process of binding up a service that your client has permission to bind to; false if the system couldn't find the service or if your client doesn't have permission to bind to it. 

`onBind`'
Return the communication channel to the service. May return null if clients can not bind to the service. The returned IBinder is usally for a compolex interface that has been described using aidl. 
Parameters:
intnet: the intent that was used to bind to the service, as given to COntext.bindService. 
return IBinder: retun an IBinder through which clients can call on the service. 

### IBinder
Base interface for remotable object, the core part of a lightweight remote procedure call mechanism designed for high perfromance when perfroming in-process and cross-porcess calls. This interface describes that abstract protocol for interacting with a remotable object. Do not implement this interface directly, instead extend from Binder. 
The Key IBinder API is transact() matche by Binder#onTransact. These methods allow you to send a call toan IBinder object and receive a call coming  in to a Binder object, respectively. This transaction API is syncrhonous, such that a call to transact() does not return until the target has returned from Binder#onTransact this is the expected behavior when calling an object that exists int eh local process, and the underlying inter-process communicatinoo (IPC) mechansim ensures that these same semantics apply when going across proceesses. 
