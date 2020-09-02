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

Context-registered recievers receive broadcasts as long as their registering context is valid. For an example, if you register withinan Activity context, you receive broadcasts as long as the activity is not destoryed. If you register with teh Applicationcontext, you receive broadcastas as longa s the app is running. 

3) To stop receiving broadcasts, call unregisterReceiver(android.content.BroadcastReceiver). Be sure to unregister teh receiver when you no longer need it or the context is no longer valid. 

Be mindful of where you register and unregister teh receiver, for example, if you register a receiver in onResume(), you should unregister it in onPause() to prevent registering it multiple times (if you don't want to receive broadcats when paused, and this can cut down on unnecessary system overhead). Do  not unregister in onSaveInstanceSTate(Bundle) becuase this isnt' called if the user moves back in the history stack
