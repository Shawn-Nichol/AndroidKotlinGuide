# How to create a Service

## 1) Create a service Class
Create a new class that extends Service and pass an empty constructor. 

```
class MyService : Service() {

}
```

2) Override the following methods

`onCreate()` <br>
The system invokes this method to preform one-time procedures when the service is initially created. If the service is already running this method isn't called. 

`onStartCommand: `<br>
The system invokes this method by calling startService() when anothe component requests that the service be started. When this method executes the servie is started and cana run in the backgroun indefinitely. If you implement this it is your responsibility to stop the servie when its work is complete by calling stop self or stop service. 

Parameters
- Intent: the intent supplied to Context.startService, as given. This may be null if the service is being restarted after its process has gone away, and it had previously returned anything.
- Flags: Additioanl data about this start requeset. Values is either 0 or a combination of START_FLAG_REDELIVER, and START_FLAG_RETRY
- startId: A unique integer rpresenting this specific request to start. 

return: the return value indicatess what semantics the system should use for the service current started state. 

`onBind: ` <br>
The system invokes thsi methdo by calling bindService() when antoher componetnn wants to bind with the service. you must provide an interface that clients use to communicate with the service by returning an IBinder. This method must always be implemented by if you don't want to allow binding return null. 
Parameters
- Intent: The intent that was used to bind tto this service, as given to bindService. Note that any extras that were included iwth the intent at that point will not be seen. 

return IBinder: return an IBinder through which clients can call on to the service. 

```
class MyService: Service() {
  
  override fun onCreate() {
    ...
  }
  
  override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
    ... 
    return START_STICKY
  }
  
  override fun onBind(intent: Intent?): IBinder? {
    return null
  }
  
}

```

## Add Service to Manifest
If the service isn't added to the manifest the service won't install. 

```
<application
  ...
  
  <service android:name=".myService"/>
</application>
```

## StartService. 
To start a service you'll need to call the Intent for the service some where in the Activity or fragment. 
```
class MyFragment: Fragment() {
  
  
  lateinit var binding: ...
  
  override onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View {
    ... 
    startMyService()
  }
  
  fun startMyService() {
    val myService = Intent(requireContext9), MyService::class.java)
    
    // THis for a toggle button to turn on and off the service see toggle button for details. 
    binding.ServiceOnServiceOff.setOnCheckedChangeListener { _, isChecked -> 
      if(isChecked) {
        requireContext().startService(myService)
      } else {
        requireContext().stopService(myService)
      }
  }
}
```

