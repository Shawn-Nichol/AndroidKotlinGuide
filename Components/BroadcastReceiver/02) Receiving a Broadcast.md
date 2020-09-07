# Receiving
Apps can receive a broadcast in two ways: Through a manifest declared receiver and context registered receivers. Manifest declared receivers are depricated as of API 24, a manifest reciever will start an activty


## Manifest Receiver
How to create a Manifest receiver 

### 1) Specify the element in your apps manifest.
```
receiver android:name=".MyBroadcastReceiver" android exported="true">
  <intent-filter>
        <action android:name="android.intent.action.BOTT_COMPLETED"/>
    <action android:name="android.intent.action.INPUT_METHOD_CHANGED" />
  <intent filterer>
</receiver>
```

Intent filters specify the broadcast action your receiver subscirbes to. 

### 2) Subclass BroadastReceiver and impelemtn onReceive(Context, Intent). 
The following broadcast receviers logs and displays the contents of the broadcast
```
class MyBroadastReceiver: BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    // Do something. 
  }
}
```

The System package manager registers the receiver when the app is installed. The receiver then becomes a seperate entry point into the app which means the the system can start the app and deliver the broadcast if the app is not currently running. The system creates a new BroadcastRecever  component object ot handle each broadcast that it receives. This object is valid only for the duration of the call to onReceive(Context, Intent). Once your code returns from this method, the system considers the component no longer active.

## Context-regsiter Receivers
How to registera receiver with a context.

### 1) Create a Broadcast Receiver
```
val br: BroadcastReceiver = MyBroadcastReceiver()
```

### 2) Create an IntentFilter
Create an IntentFiltera and register the receiver by calling registerReceive(BroadcastReceiver IntentFilter)
```
val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION)apply {
  addAction(Intent.ACTION_AIRPLANE-MODE_CHANGED)
}

registerReceiver(br, filter)
```
To register a local broadcasts, call LocalBroadcastReceiver(BroadastReceiver, IntentFIlter)

Context-registered receivers receive broadcasts as long as their registering context is valid. For an example, if youregister within an activity context, youreceive broadcasts as long as the activity  is not destrtoyed. If you register with the Application context, youreceive broadcasts as long as the app is running. 

### 3) Stop Receiving 
To stop receiving broadcasts, call unregisterReceiver(android.context.BroadcastReceiver). Be sure to unregister the receiver when you no longer need it or the context is no longer valid, be mindufl of where you register and unregister the receiver, for example, if you register in onCreate() using the activit's context, you should unregister it in onDestory() to preven tleaking the receiver out of the activvity context. If you register receiver in onResume(), you should unregsister it in onPause() to prevent registering it multiple times. 

## Effects on proccess state
The state of your BroadcastReceiver (whether its running or not) affects the state of its contiang process, which can in turn affect its likelihood of being killed by the systme. For example, when a process executes a receiver (that is currently running the code in the onReceive() method), it is considered to be a foreground process. The system keeps the process running expect under cases of extreme memory pressure. 

Once the onRecieve() finishes, the BroadcastReciever is no longer active. The receivers host process becomes only as important as the other app component that are running in it. If that process hosts only a manifest declared receiver then upon  return from onReceive(), the system considers its process to be a low-priotity process and it may be killed in order to make resources for more important processes. For this reason, you should not start long running background threads from a BroadcastReceiver. After onReceive(), the system can kill the process at any time to reclaim memory, and in doing so it terminates the spawned threading (background threads) or schedules a JobService form receiver using the JobScheduler, so the system knowns that the process continues to perfrom active work. 

A BroadcastReceiver that uses goAsync() to flag that it needds more time to finish after onReceive() is complete. This is usefule if the onRecieve() isn't long enough and might cuase you to miss a frame. 
```
class MyBraodcastReceiver : BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    val pendingResult: PendingResult = goAsync()
    val asynTask = Task(pendingREsult, intent)
    asynTask.execute()
    
    private class Task(
      private val pendingResult: PendingResult,
      private val intent: Intent
    ): AsyncTatsk<String, Int, String>() {
      override fun doInBackground(vararg parmas: String?): String) {
                    val sb = StringBuilder()
            sb.append("Action: ${intent.action}\n")
            sb.append("URI: ${intent.toUri(Intent.URI_INTENT_SCHEME)}\n")
            return toString().also { log ->
                Log.d(TAG, log)
      }
      
      override fun onPostExecute(result: String?) {
        super.onPostExecute(result)
        // Must call finish() so the BroadcastReceiver can be recycled.
        pendingResult.finish()
      }
    }
  }
}
```


# Receiving with Permissions
If you specify a permission paramater when registering a broadcast receiver (either with registerReceiver(BroadcastReciever, IntentFilter, String, Handler) or in tag in your manifest), then nonly broadcasters who have requested the permission with the tag in tehir manifest can send an Intent to the receiver. 

Receving app has a manifest-declared receiver
```
<receiver android:name=".MyBroadcastReceiver"
    android:permission="android.permission.SEND_SMS">
    
    <intent-filter>
      <action android:name="android.intent.action.AIRPLANE_MODE"/>
    </intent-filter>
</reciever>
```

The receiving app has a context registered receiver. 
```
val filter = IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED)
registerReciever(reciever, filter, Manfest.permission.SENS_SMS, null)
```
Then to be able to send a broadcast to those recievers, the sending app must request the permission. 
```
<uses-permission android:name="android.permission.SEND_SMS"/>
```
