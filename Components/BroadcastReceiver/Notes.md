# Broadcast Overview
Android apps can send or receive broadcast messages from the  Android system and other Android apps, similar to the publish-subscribe design pattern. These broadcast are sent when an event of interest occurs. For example, the android system sends broadcast when various system events occur, such as when the system boots up or the device starts charging. Apps can also send custom broadcasts, for example to notify other apps of somethign that they might be interested in.

Apps can register to reeive specific broadcasts to apps that have subscibed to reeive that particular type of broacast

Generally speaking, broadcasts can be used as messaging system across apps and outside of the normal user flow. However, you must be careful not to abuse the opportunity to respond to broadcast and run jobs in the background that can contribute to a slow system performance. 

## About system Broadcast
the system automatically sends broadcasts when various sytem events occur, such as when the system switches in and out of airplane mode. System broadcasts are sent to all apps that are subscribed to reeive the event. 

The broadcast message itself is wrapped in an Intent object whose action string identrifies the event taht occurred. The inent may also include additional information bundled into its extra field. For examlple, the airplane mode intent includes a boolean extra that indicates whether or not Airplane Mode is on

For a complete list of system broadcast actions, see the BROADCAST-ACTIONS.TXT file in the Android SDK. Each broadcast action has a constant fieldd associated with it. For example, the value of the constant ACTION_AIRPLANE_MODE CHANGED is android.intent.action.AIRPLANE_MODE. Documentation for each broadcast action is available in its associated constant field. 

## Change to system broadcasts
As the android platform evolves, it periodically changes how system broadcast behave. Keep the following changes in mind if your app targets Anrdoird 7 or higher, or if it's installed on devices running anrdoid 7 or higher. 

## Receiving a broadcast
Apps can reeive broadcasts in tow ways: through manifest-declared receivers and context-registered receivers

Manifest-declared receivers
If you declare a broadcast receiver in your manifest, the system launches your app when the broadcast is sent. 

Note: If your app targets API level 26 or higher, you cannot use the manifest to declare a receiver for implicit broadcasts(broadcasts that do not target your app sepecifically), except for a few implicit broadcasts that are exempted from that restriction. In most cases, you can use scheduled jobs instead. 

To declare a broadcast receiver in the manifest, perform the following steps
1) Specify the <receiver> element in your app's manifest. 
```
<receiver android:name=".MyBroadcastReceiver" android: Expoorted="true">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPETED"/>
    <action android:name="android.intent.action.INPUT_METHOD_CHANGED"/>
  </intent-filter>
</receiver>
```
The intent filteres specify teh broadcast actions your  receiver subscibes to

2) Subclass BroadcastReceiver and implement onReceive(context, Intent). The broadcast receiver in the following example logs and displays the contents of the broadcast:

```
private const val TAG = "MyBroadcastReceiver"

class MyBroadcastReceiver : BroadcastReceiver() {
  StringBuilder().apply {
    append("Action: ${intent.action}/n")
    append("URI: ${intent.toURI(Intent.URI_INTENT_SCHEME)}\n")
    toString().also { log ->
      Log.d(TAG, log)
      Toast.makeText(context,log, Toast.LENGTH_LONG).show()
    }
}

```

The system package manager registers teh receiver when the app is installed. The receiver then becomes a separate entry point into your app which means that the system can start the app and deliver the broadcast if the app is not currently running. The system creates a new BroadcastReciever component object to handle each broadcast that it receives. This object is valid only for the duration of the call to onReceive(Context, INtent). Once your code returns sfrom this method, the system considers teh component no longer active

### Context-registered receivers
To regiseter a receiver with a context, perform the following steps:
1) Create an instance of BroadcastReceiver
```
val br: BroadcastReceiver = MyBroadcastReceiver()(
```
2)  Create an IntentFilter and register the receiver by calling registerReceiver(BroadcastReceiver, IntentFilter)
```
val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION).apply {
  addAction(Intent.ACTION_AIRPLANE_MODE_CHANGED)
}
registerReceiver(br, filter)
```
Note to register for local broadcast, call LocalBroadcastManger.registerReceiver(BroadcastReceiver, IntentFilter)



## Effects on procces state. 
The state of your broadcastReceiver (whether it is running or not) affects the state o fits containing process, which can in turn affect it liklihood of being killed by the system. For example, when a process executesa receiver (that is, currently running the code in its onReceive() methdod), it is considered to be a foregorund process. The system keeps the process running except under cases of extrreme memory pressure. 

However, once your code returns from onReceive(), the BroadcastReciever i sno longer active. The receiver's host process becomes only as important as the other app components that are running in it. If that process hosts only a manifest-declared receiver (a common case for apps that the user has never or not recently interacted with), then upon returning from onReceive(), the system considers its process to be a low-priority process and may kill it to make resources available for other more important processes. 

For this reason, you should not start long running background threads from a broadcast receiver. After onReceive(), the system can kill the process at any time oto reclaim memory , and in doing so, it terminates the spawned thread runningin the process. To avoid this, you should either call goAsync() if you want a little more time to process the broadcast in a backgrounthread) or schedule a JobService from the receiver using the JobScheduler,so the sytme knows that the process continues to perfrom active work.

The following snippet shows a broadcastReceiver that uses goAsync() to flag that it needs more time to finish after onReceive(*) is complete. This is especially useful if the work yhou want to complete in your onReceive9) is long enough to cuase the UI thread to miss a frame(>16ms), making it better suited for a background thread.

```
private const val TAG = "MyBroadcastReceiver"

class MyBroadcastReceiver : BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    val pendingResult: PendingResult = goAsync()
    val asynTask = Task(pendingResult, intent)
    asyncTask.execut()
    
    private class Task(
      private val pendingResult: PendingResult, 
      private val intent: Intent
      ) : AsyncTask<String, Int, String>() {
        override fun do InBackground(vararg params: String?): String {
          val sb = StringBuilder9)
          sb.append("Action: ${intent.actin}\n")
          sb.append("URI: ${intent.toUri(Intent.URI_INTENT_SHCEME)}/n")
          return toString().also { log ->
            Log.d(TAG, log)
          }
        }
        
        override fun onPostExecute(result: String?) {
          super.onPostExecute(result)
          // Must call finsih() so the BroadcastReceiver can by recycled.
          pendingREsult.finish()
        }
      }
  }
}
```

## Sending a Broadcast
Android provides three ways for apps to send broadcast
- THe sendORderBroadcast(Intent, String) method sends broadcasts to one receiver at a time. AS each receiver executes in turn, it can propagate a result to the next receiverr ,or it can completely abort the broadcast so that it won't be passed to other receivers. The order receivers run in can be controlled iwth the android:priority attribute of the matching intent-filter; receivers with the sam priortiy will be run in an arbitrary order 
- The sendBraodcast(intent) method sends broadcasts to all receivers in an undefined order. This is called a NOrmal Broadcast. This is more efficeint, but means that receivers cannot read results from other receivers, propagate data received from the broadcast or abort the broadcast
- The local BrodcastManager.sendBroadcast method sends brodcasts to receiversthat are in the same app as the sender. If you don't need to send broadcasts across apps, use local broadcasts. The implementation is much more efficient(no interprocess communication needed) and you don't need to worry about any security issues related to other apps being able to receive or send your broadcasts. 

How to send a braodcast by creating an Intent
