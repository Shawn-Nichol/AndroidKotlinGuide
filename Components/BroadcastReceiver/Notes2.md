# Broadcast Overview
Android apps can send or receive broadcast messages from the Android system and other android apps, similiar to the publish-subscibe desgn pattern. These broadcasts are  sendt when an even t of interest occurs. ofr example, the android system sends broadcasts when various system events occur, such as when the system boots up or the device starts charging. Apps can also send custom broadcast, for example, to notify other apps of something that they migt be interested in 

Apps can register to receive specific  broadcasts. When a broadc ast is ent, the system automatically routes broadcasts to appss that have sbuscribed to receive that particular type of braodcast. 

Generally speaking, broadcasts can be used as a messgaging system across apss and outside of the  the normal user flow. However, you must be careful not to abuse the opportunity to respond to broadcasts and run jobs in the background that can contribute to slow system performance. 

## About system broadcasts

The system automatically sends broadcasts when various system events occur, such as when the system switches in and out of airplane mmode. System broadcasts are sent to all apss tthat are subscribed to recceive the event. The broadcast message itsefl is wrapped in Intent object whose action string identifies the event that occurred (for example, the airplane mode intent includes a boolean extra that indicates whether or not Airplane mode is on. 

For more information about how to read intents and get actions string from an intent see intents and inten t filters. 

For a complete list of system broadcasts actions, see the BROADCAST_ACTIONS.TXT file in teh Android SDK. Each broadcast action has constant field associated iwth it. For example, the value of the constant ACTION_AIRPLANE_MODE_CHANGED is android.intent.action.AIRPLANE_MODE. Documentation for each broadcast action is available in its associated constant field

## Receiving Broadcasts
Apps can receive broadcasts in two ways: Trhough manifes-ceclared receivers and context-registered receviers
Manifest-declared receivers
If you declared a broadcast receiver in your manifest, the system launches your app (if the app is not already running) when the broadcast is sent. 

To declare a broadcast receiver in the manifest, perform the following steps
1) specify the <receiver> element in your apss manifest

```
<receiver android:name=".MyBroadcastReceiver" android exported="true">
  <intent-filter>
    <action android:name="android.intent.action.BOTT_COMPLETED"/>
    <action android:name="android.intent.action.INPUT_METHOD_CHANGED" />
  <intent filterer>
</receiver>
```
Intent filters specify the broadcast actions your receiver subscribes to. 

2) Subclass BroadcastREceiver and implement onRecieve(Context, Intent). The broadcast receiver in the following example logs and displays the contents of the broadcast.
```
class MyBroadcastReceiver : BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    // Dosomething
  }
}
```
THe system package manager registers the receiver when the app is installed. The receiver then becomes a s separate entry point into your app which means that the system can start the app and deliver the broadcast if the app is not currently running. The system creates a new BoradcastReceiver component object to handle each broadcast that it receives. THis object is valid only for the duration of the call to onReceive(Context, Intent0. Once your code returns from this method, the system considers the component no longer active. 


## Context-registered receivers
To register a receiver with a context, perfrom the following steps
1) create an instance of BroadcastReceiver
```
val br: BroadcastReceiver = MyBroadcastReciever()
```

2) create an IntentFilter and register the receiver by calling registerReceiver(BroadcastReceiver IntentFilter):
```
val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION).apply {
  addAction(Intent.ACTION_AIRPLANE_MODE_CHANGED)
}

registerReceiver(br, filter)
```

Note to register for local braodcasts, call LocalBroadcastReciever(BroadcastReceiver, IntentFilter)

Context-registered receivers receive braodcasts as long as their registering context is valid. Fo an example, if you register within an Activity context, you receive braodcaasts as long as the activity is not destroyed. If you register with teh Application context, you receive broadcasts as long as the app is running. 

3) To stop receving broadcasts, call unrgisterReceiver(android.context.BroadcastReceiver). Be sure to unregister the receiver when you no longer need it or the context is no longer valid. 

Be mindful ow where you register and unregsister the receiver, for example, if you registera receiver in onCreate(Bundle) using the activity's context, you should unregister it in onDestory() to prevent leaking the receiver out of the activity context. If you registera receiver in onResume(), you should unregister it in onPause() to prevent registering it mulitple times (If you  don't want to receive broadcasts when paused, and this can cut down on unnecessary system overhead)> Do not unregister in onSaveInstanceState(Bundle), becuase this isn't called if the user moves back in the history stack. 

## Effects on process state

The state of your BroadcastReceiver (whether it is running or not affects the state of its contiang process, which can in turn affect its likelihood of being killed by the system. For example, when a process executes a receiver(that is currently running the code in the onReeive() method), it is considered to be a foreground process. The system keeps the process running expected under cases of extreme meory pressure. 

However, once your code returns from onReceive(), the BroadcastReceiver is no longer active. The reeiver's host process becomes only as important as the other app component that are running in it. If that process hosts only a manifest-declated receiver (a common case for apps that the user has never or not recently interacted with), then upon returning from onReceive(), the system considers its process to be a low-priority process and may kill it to makes resources available for other more important processes. 

For this reason, you should not start long running background threads from a broadcast receiver. After onReceive(), the systme can kill the process at any time to reclaim memory, and in doing so it terminates teh spawned threadin the background thread) or schedule a JobService from receiver using the the JobScheduler, so the system knowns that the process continues to perfrom active work.

The following snippet shows a BroadcastReceiver that uses goAsync() to flag that it needs more time to finish after onReceive() is complete. This is especially useful if the work you want to complete in youron Reeive() is ong engough to cause the UI thread to miss a frame making it better suited for a background thread. 
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

## Sending Broadcasts
Android provides three ways for apps to send broadcsts:

- The sendOrderedBroadcast(intent,String) methods edns broadcasts to one receiver at a time.As each receiver executes in turn, it can propagate a result to the next receiver, or it can completely abort the broadcast so that it won't be passed to other receivers. The order receivers run in can be controlled with the android: priority attribute of the matching intent-filter; receivers with the same priority will be run in an arbitrary order

- The sendBroadcast(Intent) method sends broadcasts to all receivers in an undefine dorder. This is called a normal Broadcast. This is mroe efficient, but means that receivers cannot read results from other receivers, propagate data received from the broadcast, or abort the broadcast

- The LocalBroadcastManager.sendBroadcast meethod sends broadcasts to receivers that are in the sam app as the sender. If you don't need to send broadcasts across apps, use local broadcasts. The implementation is much more efficient (no interporcess communication needed) and you don't need to worry about any security issues related to other apps being abole to receive or send yoru broadcasts. 

The following code snippet demonstartes how to send a broadcast by creating an intent can calling sendBraodcast(intent)
```
Intent().also { intent -> 
  intent.setAction("com.example.broadcast.MY_NOTIFICATION") 
  intent.putExttra("data", "notice me senpai!")
  sendBroadcast(intent)
```
The braodcast message is wrapped in an Intent object. The intent's action string must provide the app's Java package name systnax and uniquely idenityf the broadcast event. You can attach additional information to the intent with putExtrar(Stirng, Bundle). You can alos limit a broadcast to a set of apps in the same organization by calling setPackage(String) on the intent. 


## Restricting broadcasts with permissions
Permissions allow you to restrict broadcast to the set of apps that hold certain permissions. You can enforce restrictions on either the sender or receiver of a broadcast.

## Sending with permissions
When you call sendBroadcast(Intent, String) or sendORderedBroadcast(Inent, String, BroadcastReceiver, Handler, int, String, Bundle), you cna specify a permission parameter. ONly receivers who have requested that permission with the tag in their manifest (and subsequently been granted the permission if it is dangerous) can receive the broadcast. For example, the following code sends a broadcast
```
sendBroadcast(Intent("com.example.NOTIFY"), Manifest.permission.SEND_SMS)
```
To receive the broadcast, the receing app must request the permission as shown below
```
<uses-permission android:name="android .permission.SEND_SMS"/>

```
You can specify either an existing system permission like SEND_SMS or define a custom permission with the <permission> element. For information on permissions and security in general, see the System Permissions. 
  
Note: Current permission  are registered when the app is installed. The app that defines the custom permission must be installed befoare the app that uses it. 

### Receiving with permissions
If you specify a permissio nparameter when registering a broadcast receiver (either iwht registerReceiver(BroadcastReceiver, IntentFilter, Stinrg, Hanlder) or in <reciver> tag in you manifest), then only broadcasters who have requesteed the permission iwth the <uses-permission> tag in thier manifest (and subsequently been granted the permission if it is dangerous) can send an Intent to the receiver. 
  
Receiving app has a manifest-ceclared receiver 
```
<receiver android:name=".MyBroadastReceiver"
  android:permission="android.permission.SEND_SMS">
  
  <intent-filter>
    <action android:name='android.intent.action.AIRPLANE_MODE"/>
  </intent-filter>
</receiver>
```

receivign app has a context-registered receiver
```
val filter = IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED)
registerReceiver(receiver, filter, Manifest.permission.SEND_SMS, null)
```

Then to be able to send a broadcast to those receivers, the sending app must request the permission.
```
<uses-permission android:name="android.permission.SEND_SMS"/>
```

## Security considerations and best practices
- if you don't need to send a broadcast to components outside of your app, then send and receive local broadcasts with the LocalBroadcastManager which is available in the Support Library. The LocalBroadcastManager is much more efficient (no interprocess communicatino needed) and allows you to avoid thingking about any security issues related to other apps being able to receive or send your broadcasts. local Broadcats can be used as general purpose pub/sub event bus in your app without any over heads of system wide broadcasts. 

- If many apps have registered to receive the sam broadcast in their manifest, it can cause the system to launch a lot of apps, causing a substaintial impact on both device perfromance an user experince. To avoid this, prefer using context registration over manifest declaration. Sometimes, the Android system itself enforces the use of context-reegistered receivers. For example, the CONNECTIVITYY_ACTION broadcast is delivered only to context-registered receivers
- Do not broadcast sensitvie information using a implicit itnent the information can be read by any app that registers to receive the broadcast. There are three ways to control who can receive your broadcast.
  - You can specify a permission when sednign a broadcast
  - Android 4.0 and higher, you can specify a package with setPackage(String) when sending a broadcast. The system restirticts the broadcast to the set of apps that match the package. 
  - You can limit yourself toonly local broadcasts with LocalBroadcastManager. 
  
  The namespace of broadcast actions is global. Make sure that action names and other strings are written in a namespace you won, or else you may inadvertenly conflict with other apps. 
  - Because a receiver's onReceive(Context, Intent) method runs on the main thread, it should execute and return quickly. If you need to perfrom long running owrk, be careful about spawningin threads or starting background services becuase the system can kill the entire process after onReceive() returns. For more information, see Effecton process state to perfrom long running work, we recommend:
    - Calling goAsync() in your receiver's onReceive() method and passing the BroadcastReceiver. PendingResult to a background thread. This keeps the broadcast active after returning from onRecieve(). However, even with this approach the sytem expects you to finish with the broadcast very quickly. It does allow you to move work to another thread to avoid glitching the main thread. 
    - Scheduling a job with the JobScheduler
  - Do not start activites from broadcast receivers becuase the user experince i sjarring; especially if there is more than one receiver. Instead consider displaying a notification. 
