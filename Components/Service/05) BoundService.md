# Bound Service 
A boudn service is the server in a client server interface. It allows components to bind to the service, send requests, receive reponses, and perofrm interprocess communication (IPC). A bound service typically lives only while it serves another application component and does not run in the backgroun indefinitely. To provide binding for a service you must implement the onBind() callback method(). This method returns an IBinder object that defines the programming interface that clients can use to interact with the service. 


## Create a bound a service
When creating a service that provides binding, you must provide an IBinder that provides the programming interface that  clients can use to interact with the service. There are three ways to interface:

1) Extending the binder class
If your service is private to your own application and runs in the same process as the client, you should create your interafvae by extending the Binder class and returning an instance of it from onBind(). The client receives the Binder and can use it to directly access public methods available in either the Binder implementation or the service. 

This the preferred techinque when your service is merely a background worker for your own application. The only r reason you would not create your interafce this way is because your service is used by other application or across separate processes. 

2) Using Messenger
If you need your interaface to work across different processes, you can crerate an interface for the srevice with a Messenger. In this manner, the service defines a Handler that responds to different types of Messages objects. This Handler is the basis for a messenger that can then share an IBinder with the client, allowing the client to see commands to the service using Message objects. Additionally, the client can define a Messenger of its own, so the service can send messages back. 

Using a Messenger for your interface is impler than using AIDL becuase messenger queues all calls to the service. A pure AIDL interface sends simultaneous request to the service, which must then handle multi-threading. For most application, the service doesn't need to perform multi-threading, so using a meseenger allows the service to handle one call at a time. If it's important that your service by multi-threaaded use AIDL to define your interface. 

3) Using AIDL
Android Interface Definitaion Language(AIDL) decomposes objects into primitives that the operating system can understand and marshal them  across processes to perfrom IPC. The previous techinque using a Messenger, is actually based on AIDL as its underlying structure. As mentioned above, the Messenger creates a quuee of all the client requests in a single thread, so the service receives requests one at a time. If, hosever you want your service to handle multiple requests simultanesouly, then you can use AIDL directly. In this case, your service must be thread-safe and capable of multi-threading. 

## Binding a Service
Clients can bind to a service by calling bindService(). The android system then calls the service's onBind() method, which returns an IBinder for interacting with the service. The binding is asynchronous, and bindSErvice() returns immediately without returning the IBinder to the client. To receive the IBinder, the client must create an instance of SERviceConnection and pass it to bindSErvice(). The serviceConnection includees a cllback method that the system calls to deliver the IBinder. 

Only activities, services and content providers can bind to a service you can't bind to a service from a broadcast receiver. 
1) Impelemt SErviceConnection your implementation must override tow callbackss methods

onServiceConnected() : the system calls this to deliver the IBinder returned by the service's onBind() method

onServiceDisconnected(): The android system calls  this when the connection to the service is unexpectedly lost, sucha s when the service has crashed or has been killed. This not called when the lient ubinds. 

2) Call bindService(), passing the ServiceConnectino implementation. Note if the method returns false, your client does not have a vlaid connection to the service however your client should still call unBindService() otherwise your client will keep the service from shutting down when idle

3) When the system calls your onSErviceConnected() callback method, you can begin making calls to the service, using the methods defined by the interface. 

4) To disconnect from the service, call ubindService()If your client is still bound to a service when your app destorys the client, destruction cuases the client to still be bound to a service when your app destoryes the cleient, destruion cuases the client to unbind. It is better practice to unbind the client as soon as it is done interacting with the service. DOing so allows the idle service to shutdown.

The following example connect the the client to the service created above by extending the Binder class os all it must do is cast the returned IBinder to the LocalSErvice class and request eh LocalService instance. 

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
With the serviceConnection, ,the client can bind to a service by passing it to bindService()
```
Intent(this, LocalService:class.java).also { intent -> 
  bindService(intent, connection, Context>BIND_AUTO_CREATE)
```
- The first parameter of bindService() is an Intent that explicitly names the service to bind. 
- The second parameter is the serviceConnection object.
- the thrid parameter is a flag indicating options for the binding. It should usually be BIND_AUTO_CREATE in order to create the service if it's not arleady alive. Other possible values are BIND_DEBUG_UNBIND and BIND_NOT_FOREGROUND, or 0 for none. 

## Binder class
If the service is only used by the local application you do not need to work across processe, allowing you to implement your own Binder class that provides your client direct acces to public emthods in the service. 

1) Create an intance ob the binder that does one of the following
   - Contains public methods that the client can call
   - Returns the current Service instance, which has public methods that the client can call
   - Returns an instance of another class hosted by the service with public methods the client can call
   
2) Return the instance of Binder from the onBind callback method
3) in the cleint receive the Binder from the onServiceConnected() callback method and make calls to the bound service using the methods provided. 

A Service that provides clients with access to methods through Binder, the local binder provides the getSErvice() method for clients to retrieve teh current instance of LocalService. This allows clients to call public methods in the service. For example, cleints can call getRandomNumber() from the service. 
```
class LocalService : Service() {
  // Binder given to the clients
  private val binder = LocalBinder()
  
  // Random number generator
  private val generator = Random()
  
  // method for clients
  val randomNumber: Int
    get() = generator.nextInt(100)
    
  /**
  * Class used for the client Binder. Becuase we know this service always runs in teh same process as its clients, we don't need to deal with IPC
  */
  inner class LocalBinder: Binder {
    // Return this instance of LocalService so clients can call public methods
    fun getService(): LocalSErvice = this@LocalService
  }
  
  override fun onBind(intent: Intent): Binder {
    return binder
  }
  
}
```


An activity that binds to LocalSerivce and call getRandomNumber() whena button is clicked.
```
class MyActivity : Activity() {
  private lateinit var service: LocalService
  private var bound: Boolean = false
  
  /** Defines callbacks for the service binding, passed o bindService() */
  private val connection = object : ServiceConnection {
    override fun onServiceConnected(className: ComponentName, service: IBinder) {
      val binder = service as LocalService.LocalBinder
      service = binder.getService()
      bound = true
    }
    
    override fun onServiceDisconnected(arg: ComponentName) {
      bound = false
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
    unBindService(connection)
    bound = flase
  }
  
  /** called when a button is clicked in the layout files
  fun onButtonClick(v: View) {
    if(bound) {
      // call a method in the local service
      val num: Int = service.randomNumber
      
    }
  }
}
```

## Building to a started service
A service can be both bound and started, this is done by calling startService(), which allows the service to run and you can allow a client to bind to the service by calling beindService().

A started and bound service must be explicity stopped by calling stopSelf() or stopService().

Reasons you would use started and bound service.
- Start music in a sesrvice the user can leave the app, when the user returns to the app the activity can bind to the service to regain control of play back. 

A cleint binds to the service by calling bindService(). When it does, it must provide an implementation of SErviceConnection, which monitors the connection with the service. The return vallue of bindSErvice() indicates whether the request wservice exists and whether the cleint is permitted acceess to it. When the Android system creates the connection between the client and service, it calls onServiceConnected() on the SErviceConnection. The onServiceConnected() method includes an IBinder argument, which the client then uses to communicate with the bound service. 

You can connect multiple clients to a service simultaneously. However, the system caches the IBinder service cummunication channel. In other words, the system calls the servic's onBind() method to generate the IBinder only when the first cleient binds. The system then deliverse the same IBinder to all additional clients that bind to that same service, without calling onBind() again. When the last client unbinds from the service, the system destorys the service, unless the service was also started by startedService().

The most important part of you bound service implementation is definning the interface taht your onBind() callback method returns. 



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
