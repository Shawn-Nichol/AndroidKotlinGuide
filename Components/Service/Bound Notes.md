# Bound service
A boudn service is the server in a client-server interface. It allows componets to bind to the service, send requets, receive responses, and perform interprocess communication IPC. A bound service typically lives only while it serves another application component and does not run in the background indefinitely. 

## The basics
A bound service is an implementation of the service class that allows other application to bind to it and interact with it. To provide binding for a service, you must implement the onBind() callback method. This method returns an IBinder object that defines the prograrmming interface that clients can use to interact with the service. 

## Building to a started service. 
You can create a service that is both bound and started. You can call a service by calling startService(), which allows the service to run and you can allow a client to bind to the service by calling bindService()

A service that is started and bound must be explicitly stopped by calling stopSelf() or stopService.

Sometimes its necessary to implement both onBind() and onStartCommand(). For example, a music player might find it useful to allow its service to run indefinitely and also provide binding. This way, an activity can start the service to play some music and the music cotinues to play even if the  user leaves the application. Then when the user returns to the application, the activity can bind to the service to regain control of playback. 

A client binds to the service by calling bindService(). When it does, it must provide an implementation of ServiceConnection, which monitors the connection with the service The reutrn value of bindService() indicates whether the request service exists and whether the client is permitted access to it. When the Android system creates the connection between the client and service, it calls onServiceConnected() on the ServiceConnection. The onServiceConnected() method includes an IBinder argument, which the client then uses to communicate with the bound service. 

You can connect multiple clients to a service simultaneously. However, the system caches  the IBinder service communication channel. In other words, the system calls the service's onBind() method to generate the IBinder only when the first client binds. The system then deliverse the same IBinder to all additional clients that bind to that same service, without callingn onBind() again. 

When the last client unbinds from the srvice, the system destroys the service, unless the service was also started by startedService().

The most important part of your bound service implementation is defining the interface that your onBind() callback method returns. 

## Create a bound service
When creating a service that provides binding, you must provide an IBinder that provides the programming interface that clients can use to interact with the service. There are three ways to interface:

### Extending the Binder class
If  your service is private to your own application and runs inthe same process as the client, you should create your interface by extending the Binder class and returning an instance of it from onBind(). The client receives the Binder and can use it to directly access public methods available iin either the Binder implementation or the service. 

This is the preferred teechinque when your service is merely a background worker for your own application. The only reason you would not create your interface this way is becuase your service is used  b other application or across separate processes. 

### Using a Messenger
If you need your interface to work across different processes, you can create an interface for the service with a Messenger. In this manner, the service defines a Handler that responds to different types of Message objects. This Handler is the basis for a messenger that can then share an IBinder with the client, allowing the client to seen commands to the service using Message objects. Additionally, the cleint can define a Messenger of its own, so the service can send message back

This is the simplest way to perform interprocess communication IPC, becuase the MEssenger queues all requests into a single thread so that you don't have to design your service ot be thread-safe. 


### Using AIDL
Android Interface Definition Language (AIDL) decomposes objects into primitives that the operating system can understand and marshals them across processes to perform IPC. THe previou stechinque using a Messenger, is actually based on AIDL as its underlying structure. As mentioned above, the Messenger creates a queue of all the client requests in a single thread, so the service receives requesets one at a time. If, however you want your service to hanld multiple requets simultaneously, then you can use AIDL directly. In this case, your service must be thread-safe and capable of multi-threading.  


## Extending The binder class. 
If the service is only used by the local appliation you do not need to work across processes, allowing you to implment your own Binder class that provides your client direct access to public methods in the service. 

1) In your service creae an instance of Binder that does  one of the following
  - Contains public methods taht the client can call
  - Returns the current Service instance, which has public methods the client can call
  - Returns an instance of another class hosted by the service with public methods the client can call. 
  
2) Retun the instance of Binder fomr the onBind callback method
3) In the client receive the Binder from the onServiceConnected() callback method and make calls to the bound service using the methods provided. 

A service that provides clients with access to methods through Binder
```
class LocalService : Service() {
  // Binder given to the clients
  private val binder = LocalBinder()
  
  // Random number generator
  private val mGenerator = Random()
  
  /** methods for clients */
  val randomNumber: Int
    get() = mGenerator.nextInt(100)
    
  /**
   * Class used for the client Binder. Becuase we know this service always runs in the same process as its clients, we don't need to deal with IPC. 
  */
  inner class LocalBinder : Binder() {
    // Return this instance of LocalService so clients can call public methods.
    fun getService(): LocalService = this@LocalService
  }
  
  override fun onBind(intent: Intent): Binder {
    return binder
  }
}
```
The local binder provides the getService9) method for clients to retrieve the current instance of LocalService. This allows clients to call public methods inthe service. For example, cleints can call getRandomNumber() from the service

An activity that binds to localSErvice and calls getRandomNumber9) when a button is clicked. 
```
class BindingActivity : Activity() {
  private lateinit var mService: LocalService
  private var mBound: Boolean = false
  
  /** Defines callbacks for service binding, passed to bindService() */
  private val connection = object : ServiceConnectino {
     override fun onService Connected(className: ComponentName, service: IBinder) {
      val binder = service as LocalService.LocalBinder
      mService = binder.getService()
      mBound = true
     }
     
     override fun onServiceDisconnected(arg0: ComponentName) {
      mBound false
     }
  }
  
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.main)
  }
  
  override fun onStart() {
    super.onStart()
    // Bind to local service
    Intent(this, LocalService::class.java).also { intent ->
      bindService(intent, connection, Context.BIND_AUTO_CREATE)
    }
  }
  
  override fun onStop() {
    super.onStop()
    unbindService(connection)
    mBound = false
  }
  
  /** called when a button is clicked in the layout file attaches to thismehotd with the andoird: onClick attribute*/
  fun onButtonClick(v: View) {
    if(mBound) {
      // Call a method from the LocalService
      // However, if this call were somethign that might hang, then this request should occur ina separate thread to avoid slowing down the activity performance
      val num: Int = mService.randomNumber
      Toast.makeText(this, "number: $num", Toast.LENGTH_SHORT).show()
    }
  }
}
```

Clietns should unbind from services at appropriate times, as discussed in Additional notes. 

## using a Messenger
If you need your service to communicate with remote processes, then you can use a Messenger to provide the interface for your service. This techinque allows you to perform interprocess communicatin (IPC) without the need to use AIDL.

Using a Messenger for your interface is simpler than using AIDL becuase messenger queues all calls to the service. A pure AIDL interafce sends simultaneous requests to the service, which must then handle multi-threading. 

For most applications, the service doesn't need to perfrom multi-threading, so uisng a Messenger allows the service to handle one call at a time. If it's important that your service by multi-threaded use AiDL to define your interface

TODO


## Binding to a service
Application componets(clients) can bind to a service by calling bindService(). The Android system then calls the service's onBind() method, which  returns an IBinder for interacting with the service. 

The binding is asynchronous, and bindSErvice() returns immediately without returning the IBinder to the client. To receive the IBinder, theclient must create an instance of ServiceConnection and pass it to bindService(). The ServiceConnection includes a callback method that the sysem calls to deliver the IBinder. 

Onlyl activites,servicdes, and content providers can bind to a service you can't bind to a service from a broadcast receiver. 

To bind to a service form your client, follow these steps;

1) Implement ServiceConnection
Your implementation must override two callback methods

### onSerrviceConnected()
The system calls this to deliver the IBinder returned by the service's onBind() method

### onSesrviceDisconnected()
The Android system calls this when the connection to the service is unexpectedly lost, such as when the service has crashed or has been killed. This  is not called when the clients unbinds. 

2) call bindService(), passing the ServiceConnection implementation. 
Note: If the method returns false, your client does not have a valid connection ot the service  However your client should still call unBindService() otherwise your client will keep the service from shutting dow when it is idle

3) When the system calls your onServiceConnected() callback method, you can begin making calls to the service, using the methods defined by the interface

4) To disconnect form the service, call unbindService()

If your client is still bound to a service when your app destorys the client, destructioncauses the client is still bound to a service when your app destorys the client, destructioncauses the client to unbind. It is better practice to unbind the client as soon as it is done interacting with the service. Doing so allows the idle service to shut down. For more information about appropriate times to bind and unbind see additional notes. 

The following example connects the client to the service created above by extending the Binder class, so all it must do is cast the reurned IBinder to the local SErvice class and request the LocalService instance.

```
var mSersvice: LocalService

val mConnection = object : ServiceConnection {
  // Called when the connection with the service is established
  override fun onServiceConnected(className: ComponentName, service: IBinder) {
    // Becuase we have bound to an explicit service that is running in our own process, we can cast its IBinder to a concrete class and directly access it. 
    val binder = service as LocalService.LocalBinder
    mService = binder.getService()
    mBound = true
  }
  
  // called when the connection with the service disconnects unexpectedly
  override fun onServiceDisconnected(className: ComponentName) {
    mBound = false
  }
} 
```
With this ServiceConnection, the client can bind to a service by passing it to bindService(), 
```
Intent(this, LocalService::class.java).also { intent -> 
  bindService(intent, connection, Context.BIND_AUTO_CREATE)
```
- The first parameter of bindSErvice() is an Intent that explicitly names the service to bind. 
- The second parameter is the ServiceConnection object
- The third parameter is a flag indicating options for the binding. It shouldd usually be BIND_AUTO_CREATE in order to create the service if it's not already alive. Other possible values are BIND_DEBUG_UNBIND and BIND_NOT_FOREGROUND, or 0 for none.


## Addition Notes
Here are som important notes about binding to a service
- You should always trap DeadObjecException exceptions, which are thrown when the connection has broke. This is the only exception throw by  remote methods. 
- Objects are reference counted across processes. 
- You usuallly pair the binding and unbindg during matching bring-up and tear-down moments of the clients lifecycle, as described in the following examples
  - If you need to interact with the service only while your activity is visible, you should bind during onStart() and ubnidn during onStop()
  - If you want your activity to recieve responses even while it is stopped in the background, then you can bind during onCreate() and ubnidn during onDestory(). Beware that this implies that your activity needs to use the service the entire time it's running(even in the background), so if the service is in another process, then you increase th weight of the process and it becomes more likley that the system will kill it. 
  
  
## Managing the lifecycle of a bound service. 
When a service is unbound from all clients, the android system destorys it(unless it was also started with a startSErvice call. As such you don't have to manage the lifecycle of your service if it's purley a bound service-the Android system manages it for you based on whether it is bound to any clients. 

However, if you choose to implement the onStartCommnad() callback method, then you must explicitly stop the service, because the service is now considered to be started. In this case, the service runs until the service stops itself with stopSelf() or another component calls stopService(), regardless of whether it is bound to any clients. 

Additionally, if your service is started and accepts binding, then when the system calls your onUnbind() method, you can optionially return true if  you wouuldl like to receive a call to onRebind() the next time a client binds to thte service. onRebind() returns void, but the client still receives the IBinder in its onServiceConnected() callback. The following figure illustrates the logic for this kind of lifecycle. 
