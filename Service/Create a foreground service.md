## Create service class
Create a new class that extends Service()
```
class MyForegroundService : Service() {

}
```


## Overide methods. 
Override the following methods in the service class

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
class MyForegroundService: Service() {
  
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

## Notification
Foreground services require notification,in the onCreate method add a notificaion

```
class MyForegroundService : Service() {
  override fun onCreate() {
    val intent = Intent(this, MainActivity::class.java)
    val pendingIntent = PendingIntent.getActivity(this, 0, intent, 0)
    
    val notification = Notification.Builder(this, CHANNEL_ID).apply {
      setContentTitle("Title")
      setContentText("Text")
      setSmallIcon(R.drawable.myIcon)
      setContentIntent(pendingIntent)
    }
    
    startForeground(2, notification.build())
    
  }
}
```

## Manifest
The service compoent needs to registered in the manifest in order for it to load. 
```
<application>

  <service android:name=".myForegroundService"/>
</application>
```

## Start/Stop Service
In the Activity or Fragment you will need load the notification channel before starting or stopping the service. 

```
...

fun foregroundServiceControl() {
  MyNotificationChannel(requireContext())
  
  val foregroundIntent = Intent(requireContext(), MyForegroundService::class.java)
  
  // This example uses binding and a toggle button to turn on and off the service. See binding and toggle button for more details.
  binding.btn_toggle.setOnCheckedChangeListener {_, isChecked -> 
    if(isChecked) {
      requireContext.startForegroundService(foregroundIntent)
    } else {
      requireContext.stopService(foregroundIntent)
    }
  }
}


```


