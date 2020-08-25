# Services Overview
A service is an application component that can perform long-running operations in the background. it does not provide a user interface. Once started, a service might continue running ofr some time, even after the user switches to another application. Additionally, a component can bind to a service to interact with it even perform interprocess communication (IPC). For example, a service can handle network transactions, play music, perform file I/O, or interact with a content provider, all fromthe background. 

Note
A service runs in the main thraed of its hosting process the service does not create its own thread and  dows not run ina separate process unless you specify oetherwise. you should run any blocking poerations on a serparte thread within the service to avoid application not responding ANR errors. 


There are three types of services

## Foreground  
A foreground service performs some operations that is noticieable to the user. For example, an audio app would use a foreground service to play an audio track. Foreground services must display a Notification. Foreground services contineue running even when the user isn't interacting with the app. 

## Background 
A background service performs an operation that isn't directly noticed by the user.  For example, if an app used service tocompact its storage, that would usually be a background service. 


## Bound
A service is bound when an application component binds to it by callingn bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, receive results and even do so across processes with IPC. A bound service runs only as long as another application component is bound to it. Multiple components can bind to the service at onece, but when all  of them unbind the service is destroyed. 

Service can also be started and also allow binding. it's simply a matter of whether you implement a couple of callbacks methods: onStartCommand() to all components to start it and onBind() to allow binding. 

Regardless of whether your service is started, bound or both, any application componet can use the service in the same way  that any component can use an activity by starting it witha an intent. However you can declare the service as private in teh manifest file and block access from other applications. 

## THe basics
To create a service, you must create a subclass of Service or use on of its existing subclasses. in you rimplememntation,  you must override some callback methods that handle key aspects of the service, if Appropriate. These are the most important callback methods that you should override. 

onStartCommand()
The system invokes this method by calling startSErvice() when another component requests that the service be started. When this method executes, the service is started and can run in the background indefinitely. if you implement this, it is your responisbility to stop the service when its work is complete by calling stopSelf() or stopService(). If you only want to provide binding, you don't need to implement tis method. 

onBind()
The system invokces this method by calling bindService when another cmoponent wants to bind twith the service. In your implementation of this method, you must provide an interface that clients use to communicate with ther service by returing an IBinder. You must always implement this method; however if you onnd't want to allow binding, you should return null. 

onCreate()
The system invokes this mehtod to perform one-time setup procedures when the service is initially created. If the service is already running this method is not called. 

onDestroy()
The system invokes this method when the service is no longer used nd is being destroyed. your service should implement this to clean up any resources such as threads, registered listeners, or receivers. This is the last call taht the service receives. 

If a component starts the service by calling startService() the service continues to run until it stop itself with stopSelf() or another component stop it by calling stopService(). 

If a component calls bindService() to create the service and onStartCommand() is not called, the service runs only as long as teh component is bound to it. After the service unbound from all of its clients, the ssystem destorys it. 

The Android systme stops a service only when memory is low and it must recover system resources for thactivity that has user focus. If the service is bound to an activity that has user focus, It's less likely to be killed the service is declared to run in the foreground its rearley killed. If the service is started and is long-running, the system lowers its position in the  list of background tasks overtime, and the sergvice becomes highly susceptible to killing if your service is started, you must design it gracefully hanlde restarts by the systme. if the systme kills your service, it restart is as soon as resources become available, but this also depends on the value that your return from onStartCommand().

## Declaring Service in the manifest. 
You must declare all services in your applications manifest file, just as you do for other components. To declare a service, add a <service> element as a child of <application> element. 

```
<manifest ...> 
  <application ...> 
    <service android:name=".ExampleService"/>
    ...
  </application>
</manifest>
```

ThereThere are other attributes that you can include in the <service> element to define properties such as the permissions that are required to start the service and the process in which the service should run. The android: name attribute is the only requrired attribute it specifies the class name of the service. After you publish your application, leave this name unchanged to avoid the risk of breaking code due to dependence on explicit intnetns to start or bind the service

caution
To ensure that oyur app is secure, always use an explicit itnent when starting a SErvice and ond't declare intent filters for your services. using an implicit intent to start a service is a security hazard becuase you cannot be certain of the service that responds to the intent, and the user cannot see which service starts.

you can ensure that your service is avialable to only your app by including the android:exported attribute and setting it to flase. This effectively stops other apps from starting your service, even when using an explicit intent. 

Note:
Users can see what services are running on their device. If they see a service that they don't recongize or trust, they can stop the service. In order to avoid having your service stopped accidentally by users, you need to add the android:description attribute to the service> element in your app manifest. In the description, provide a short sentence explaining what the service does an what benefits it provides. 

## Create a started service
A started service i sone that another component starts by calling startService(), which results in a call to th4e servicee's onStartCommand().

When a service is started,it has a lifecycle that's independent of teh component that started it. The service can run in the background indefinitely, even i the component that started it is destroyed. As such the service should stop itself when its job is complete by calling stopSelf(), or another componeneet can sstop it by calling stopService(). 

An application component such as an activity can start the service by calling startService() and passiing an intent that specifies the service and includes any data for the service to use. The service recieves this intent in teh onStartcommand(). 

caution
A service runs in the same process as the applicationin which it is declared and in the main thread of that application by default. If your service performs internsive or blocking operations while the user interacts with an activity from the same application, the service slows down activity performance. To avoid impacting application performance, start a new thread inside the swervice. 

You can use JobIntentService as a replacement for IntentService. 

## Extending he service class
You can extend the Service class to handle each incoming intent. Here's how a basic implementation might look: 

onStartCOmmand() method must return an integer. The integer is a vlue that descibes how the system should continue the service in the event that the system kills it. The return value from onStartCommand() must be one of the following constants

START_NOT_STICKY
If the system kills the service after onStartCommand() returns, do not recreate the service unless there are pending intents to deliver. This is the safest option to avoid running your service wehen not necessary and when your a pplication can simply restart any unfinsiheed jobs. 

START_STICKY
if the system kills the service after onStartCommand() returns, recreate the service and call onStartCommand(), but do not redeliver the last intent. Instead, the system calls onStartCOmmand() with a null intent unless there are pending intents to start the service. In tehat case, those intents are delivered. Tthis is suitable for media players that are not executing commands but are running indefinitely and waiting ro a job. 

START_RECELIVERE_INTENT
If the system kills the service after onStartCommand() returns, recreate the service and call onStartCommand() with the last intent that was delivered to the serivice. Any pending intents are delivered in turn. This is suitable for services that are actively performing a job that should be immediately resumed, sucha s downloading a file. 


## Starting a Service
You can start a service from aan activity or other appication component by passing an intent to startService() or startForegroundServie(). The android system calls  the service's onStartCommand() method and passes it the Intent, which specifies which services to start. 

An activity can start a service using an explicit intent with startService()
```
Intent(this. HelloService::class.java).also { intent -> 
  startService(intent)
}
```

The startService() method returns immediately, and the Android system calls teh service's onStartCommand() method. If the service isn't already running, the systme first calls onCreate(), and then it calls onStartCommand(). 

If the service doesn't also provide binding, the intent that is delivered with startService() is the only mode of communication between the application component and the service. However, if you want the service to send a result back, the client that start the service can create a Pending Intent for a broadcast(with getBroadcast()) and deliver it to the service in the Intent that starts the service. The service can then use the broaddcast to deliver a result. 

Multiple requests to start the srvice result in multiple corresponding calls to the service's onStartCommand(). However, only one request to stop the service( withstopSelf() or stopService()) is required to stop it. 

## Stopping a service. 
A started service must manage its own lifecycle. That is the systme doesn't stop or destroy the service unless it uust recover system memory and the service continues to run after onStartCommand() returns. The service must stop itself by calling stopSelf(), or another component can stop it by calling stop SErvice().

Once requested to stop with stopSelf() or stop Service(), the system destoryes the service as soon as possible. 

If your service handles multiple requests to onStartCommand() concurrently, you shouldn't stop the service when you're done processing a start request, as you might have  received a new start request stopping at the end of the first request would terminate the second one). To avoid this problem, you can use stopSelf(int) to ensure that your request to stop the service is alwaysbased on the most recent start request. That is, when you call stop Self(int), you pass the ID of the start request (the startId delivered to onStartCommand()) to which your stop request corresponds. Then, if the service receives a new start request before you are able to call stopSelf(int), the ID doesn't match and the service doesn't stop. 

## Creating a Boundn service

A bound service is one that allows application components to bind to it by calling bindService() to createa a long-standing connection. It generally doesn't allow components to start it by calling startService()

Create a bound service when you want to interact with the service from activities and other components in your application or to expose some of your applications functionality to other application through interprocess communication(IPC)

To create a bound service, implement the onBind() callback method to return an IBinder that defines the interface for communication withthe service. Other application components can then call bindService() to retrieve the interface and begin calling methods on teh service. The service lives only to serve the application component that is bound to it, so when there are no components bound to thervice, the system destroys it. You don not need to stop a bound service in teh same way that you must when the service is tarted through onStartCommand()

To create a bound service, you must define the interface that specifies how a client can communicate withthe service. This interface between the service and a client must be an implementation o fIbinder and is what your service must return from the onBind() callback method After the client recieves the Ibinder, it can begin interacting with the serice throughthat interface. 

Multiple clients can bind to the service ismultaneously. When a client is done interacting with the service, it calls unbindSErvice() to unbind. When there are no client bound to the service, the system destorys the service. 

There are multiple ways t implement a bound service, and the implementation is more complicated than a started service. For these reasons, the bound service discussion appearsr in a separate document about Bound Services. 

## Sending Notification to the user
When a  service is running, it can notify the user of events usign Toast Notification or Status Bar Notification. A toast notification is a message taht appears on the surface of the current window for only a momment before disappearing. A status bar notification provides an icon in the status bar with a message, which the user can select in order to take an action(such as start an activity)

Usually, a status bar notification is the best technique to use when background work such as a file download has completed and the user can now acto on it. When the  users selecte the notification from the expanded view, the notification can start an activity

## Running a service in the foreground
A foreground service is a service that the user is actively aware of and isn't a candidate for the system to kill when low on memory. A foreground service must provide a notification for the status bar, which is placed under the Ongoing heading. This means that the notification cannot be dismissed unless the service is either stopped or removed from the foreground. 

For example a music player that aplays music from a service should be set to run in the foreground, becuase the user is explicityly aware of its operation. The notification in the stats bar might indicate the current song and allow the user to launch an activity to interact with the music player. Similarly an app to let users track their runs would need a foreground service to track the uer's location.

To request that your service run in the foreground, call startForeground(). This method takes two parameters: an integer that uniquely identifies the notification and teh Notification for the status bar. The notification must have a priority or PRIORITY_LOW or higher.

```
val pendingInent: PendingINtent = 
  Intent(this, ExampleActivity:class.java).let { notificationINtent -> 
   PendingInent.getActivity(this, 0, notificationIntent, 0)
  }
val notification: Notification = Notification.Builder(this, CHANNEL_DEFUALT_IMPORTANTANCE) 
  .setContentTitle(getText(R.string.notification_title))
  .setContentText(getText(R.string.notification_message))
  .setSmallIcon(R.drawable.icon)
  .setCOntentIntent(pendingInent)
  .setTicker(getText(R.string.ticker_text))
  .build()
  
  startForeground(ONGOING_NOTIFICATION_ID, notification)
  
```
To remove the service from the foreground, call stopForeground(). THis method takes a boolean, which indicates whether to remove the status bar notficiation as well. This method does not stop the service>However, if you stop the service while it's stilll running in the foreground, the notification is alos removed

## Managing the lifecycle of a service
The lifecycle of a service is much simpler than that of an activity. However, it's even more important that you pay close attention to how your service is created and destoryed becuase a service can run in the background without the user being aware. 

The service lifecycle- from when it's created to when it's destoryed can follow either of these two plaths

Start Services
The service is created wehn another component calls startSErvice(). The service then runs indefinitely an dmust stop itself by calling stopSelf(). Another component can also stop the service by calling stopService(). When the service is stopped, the system destorys it

Bound Service
The service is created when another component calls bindServicee9). THe clien tthen communicates with the service through a IBInder interface. The client can close the connection by calling unbindService(). Multiple clients can bind to the same service and when all of them unbind, the system destorys the service and when all of them unbind, the system desotrys the service. THe service does not need to stop itself. 

These two paths aren't entierly separate. YOu can bind to a service that is already started with startService. For examlple you cn start a backgroun music service by calling startSErvice() with an Intent that identifies the music to play later, possibly when the user wants to exercise some control over the player or get infomrations about the current song, an activity can bind to the service by calling beind service. Incases such as this, stopService() or stopSelf() doesn't actually stop the service until allo fthe client unbind. 

## Implementing the lifecycle callbacks
Like an activity, a service has lifecycle callback methods that you can implmenent to monitor changes in the servic'es state an perform work at the appropriate times. The following skeleton services demsonstartes each of the lifecycle methods. 


```
class ExampleService : Service() {
// Indicates how to behave if the service is killed
private var startMode: Int = 0
// interface for clients that bind 
private var binder: IBinder? = null
// indicates whether onRebind should be used
private var allowRebind: Boolean = false

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


