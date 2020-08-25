# Managinng LifeCycle of a Service
The lifecycle of a service is much simplier than that of an activity. 

## Start Service
The service is created when another componet calls startService(). The service thenr uns indefintely and must stop itself by calling stopSelf(). Another component can alos stop the service by calling stopService(). When the service is stopped the system destorys it. 

## Bound service
The services is created wehen another component calls bindServices(). The client then communicates with the service through a IBinder interface. The client can close the connection by calling unbindSErvice(). Multiple clientts can bind to the same service and wehn all tof them unbind, the system destorys the service . THe service does not need to stop itself. 


These two paths aren't entierly sparate. You can bind to a service that is already started with startSErvice. For example you can start a background music service by calling starService() with an Intent that identifies the music to play later, possibly when the user wants to exercise some control over the player or get information about the current song, an activity can bind to the service by calling bindService. Incase such as this, stopSErvice() or stopself() doesn't actually stop the service until all the clioent unbind. 


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

If a component starts the service by calling startService the service continues to run until it stopself(() or another component stops it by calling stopServices(). 

If a component calls bindService() to create the service and onStartCommand() is not called, the service runs only as long as the componet is bound to it. After the service unbound from all of its clients, the system destorys it. 


