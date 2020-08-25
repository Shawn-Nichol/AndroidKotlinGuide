

## onStartCommand()
the system invokes this method by calling startService9) when another componnet requests that the service be started. When this methodexecutes, the service is started and can run in the backgroun indefinitely. If you implment this, it is your responisbility to stop the service when its work is completeed by calling stopSelf() or stopService(). If you only want to provide binding, you don't need to implement this method. 

## onBind()
The system invokes this method by callign bindService when another component wants to bind with the service. In you r implementation of this method, you must provide interface that clients usee to communicate with there service by returning an IBinder. You must always implement this method; however if you don't want to allow binding, you should return null

## onCreate()
The system invokes this method to perform one-time setup procedures when the service is initially created. If the service is already running this method is not called. 

## onDestory()
The system invokes this method when the service is no longer used and is being destoryed, your service shoudl implement this to clean up any resources such as trheads, registered listeners, or recievers. This is the last call that the service receives. 

If a component starts the service by calling startService the service contineus to run until it stop itself with stopself(() or another component stop it by calling stopServices(). 

If a component calls bindSErvice() to create the service and onStartCommand() is not called, the service runs only as long as the componet is bound to it. After the service unbound from all of its clients, the system destorys it. 

The Android system only stops the service when memory is low and it must recoversystem resources for the activity that has user focus. If the service is bound to an activity that has user focus, it's less likley to be killed the service is declared to run in the foreground its rearly killed. If the service is tarted and is long=running, the system lowers its position in the list of the background task overtime, and the service becomes highly susceptible to killin gif your service is started, you must design it gracefully to handle restarts by the system. If the system kills your service, it restarts as soon as resources beomce available, but this alos depends on the value that your return from onStartCommand()
