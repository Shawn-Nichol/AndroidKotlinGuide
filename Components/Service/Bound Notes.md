Abound service is the server ina lient-server interface. it allows componets to bind to the service, send reqests, reeive responses, and perform interprocess communication IPC. A bound service typically lives only while it serves another application component and does not run in the backgroun indenfinitely. 

# Basics
Abound service is an implementation of the SErvice class that allows other application to bind to it and interact with it. To provide binding for a service, you must implement the onBind() callbck method. This method returns an IBinder object that defines the programming interface that clients can use to interact with the service

Binding to a started service
As dicscussed in the Services document,  you can create a service that is both started and boudn. That  is, you can start a service by calling startService(), which allows the service to run indefinitely, and you can also allow a client to bind to the service by calling bindService().

If you do allow your service to be started and bound, then when the srice has been started, the system does not destroy the service when all client unbind. Instead you must expliciltly stop the service by calling stopSelf() or StopService().

Although you usually implement either onBind() or onStartCommand(), it's sometimes necessary to implement both. For example, a music player might find it useful to allow its service to run indefinitely and also provide binding. This way, an activity can start the service t o play some music and the music continues to play  even if the user leaves the application. Then, when the user returns to the application, the activity can bind to the service to regain control o f playback. 

A client bind to a service by calling bindService(). When it does, it must provide an implementation of SErviceConnection, which monitors the connection with the service. The return value of bindService() indicates whether the requested service exists and whether the client is permitted access to it. When the Android system creates the connection between the client and serevice, it calls onServiceConnected() on teh ServiceConnection. The onServicedConnected() method includes an IBinder argument, which the client then uses to communicate with the bound service. 

you can connect multiple clients to a service simultaneously. However, thesystme caches the IBinder service communication channel. In other words, the system calls the service's onBind() method to generate the IBinder only when the first client binds. The system then delivers that same IBinder to all additional clients that bind to that same service, without calling onBind() again. 

When the last client unbinds from the service, the systme destorys the service, unless the service was also started by startService(). 

The most important part of your bound srvice implementation is defining the interface that your onBind() callback method returns. 


## Create a bound service
When creating a service that provides binding, you must provide a IBInder that provides the programming interface that clients can use to interact with the service. There are threee ways you can define the interface

Extending the Binder class
If you service is private to your own application and runs in the same process as the client, you should create your interface by extending the Binder class and returning an instance of it from onBind(). The client recieves the Binder and can use it to directly access public methods available in either the Binder implementation or the service.

This is the preferred techinque when your service is merely a background worker for your own application. The only reason you would not create your interface this way is becuase your service is used by other application or across separatae processes. 

### Using a messenger
If you need your interface to work across different processes, you can create an interface for the service with a Messenger. In this  manner, the service defines a Handler that respoonds to different types of Message objects. This Handler is the basis for a Meessenger that can then share a IBinder with the client, allowing the client to send commands to the service using Message objects. Additionally, the client can define a messenger of its own, so the service can send messages back. 

This is the simplest way to perform interprocess communication, becuase the Messenger queues all requests into a singl e thread so that you don't have to design your service to be thread-safe. 

Using AIDL 

ANdroid Interface Definition  Language decomposes objects into primitives that the operating system can understand and marshals them across process to perform IPC the  previous techinque, using a MEseenger, is actually based on AIDL as its underlying structure. As mentiond above, the Messenger creates a queue of all the client requests in a single thread, so the service receives requests on at a time. If, however you, want your service to handle multiple requets simultaneously, then you can use AiDL directly. In this case, your service must be thread-safe and capable of multi-threading. 

To use AiDL directly, you must create an .aidl file that defines the programming interface. The android SDK tools use this file to fenerate an abstract class that implements the interface and handles IPC, which you can then extend within your service. 
