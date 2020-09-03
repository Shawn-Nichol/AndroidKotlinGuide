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
