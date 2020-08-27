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
