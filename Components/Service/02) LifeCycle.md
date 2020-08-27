# Managing a LifeCycle of a Service
A service is created when another componet calls startService(). The service then run s indefintely and must stop itself by calling stopSelf(). Another component can also stop the service by calling stopService(). When the service is topped the system destroys it. 

If a componet calls bindService() to create the srevice andd onStartCommand() is not called, the service runs only as long as the componet is bound to it. After the srevice is unbound from all of its clients, the system destroys it. 


```
override fun onCreate() {
  // The service is being created
}

override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
  // The service is starting, due to a call to startService()
  return mStartMode
}

override fun onBind(intent: Intent): IBinder? {
  // A client is binding to the service with bindSErvice()
  return mBinder
}

override fun onUnbind(intent: Intent): Boolean {
  // all Clients have unbound with the unbindService()
  return mAllowRebind
}

override fun onRebind(intent: Intent) {
  // A client is binding to the service with bindService(), after onUnbind() has already been called
}

override fun onDestory() {
  // The service is no longer used and is being destoryed. 
}
```

# Extending the Service class
You can extend the service class to handle each incoming intent. Here's how a basic implementationmight look:

onStartCommand() method must return an integer. The integer is a value that desribes how the system should continue the service in the event that the system kills it. the return value from onStartCommand() must be one of the following. 

## START_NOT_STICKY
If the system kills the service after onSTartCommnad() returns, do not recreate the service unless there are pending intents to deliver. This is the safest option to avoid running your service when not necessary and when your application can simply restart any unfinished jobs. 

## START_STICKY
if the system kills the service after onStartCommand() retuns, recreate the service and call onStartCommand(), but do not redilver the last intent. Instead the system calls onStartCommand() with a null intent unless there are pending intents to start the service. In that case, those intents are delivered. This is suitable for media players that are not executing commands but are running indefinitely and waiting for a job. 

## START_REDELIVERE_INTENT
If thes sytem kills the service after onStartCommand() returns, recreate the service and call onStartCommand() with the last intent that was delivered to the service. Any pending intents are delivered in turn. This is suitable for services that are actively performing a job that should be immediately resumed suuch as a downloading a file. 

