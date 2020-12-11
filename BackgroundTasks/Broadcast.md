# Broadcasts
Android apps can send or receive broadcast messages from the Android system and other andorid apps, similar to the publish-subscirbe design pattern. These broadcastas are sent when an event of interest occurs. For example, the android system sends broadcasts when various system events occur, such as when the system boots up or the device starts chargin. Apps can also send custom broadcasts, for examplle, to notify other apps of somethign that they might be interested in (for example, some new ddata has been downloaded)

Apps Can register to receive specific broadcasts. Wehen a broadcast is sent, the system automatically routes broadcats to apps that have subscribed to receive that particular type of broadcast. 

Generally speaking, broadcats can be used as a messagining system across apps and outside of the normal user flow. However, you must be careful not to abuse the opportunity to respond to broadcats and run jobs in teh background that can contribute to slow system perfromane. 

## About system broadcasts
The system automaticallly sends broadcats when various sytem events occurs, such as when the system switches in and out of airpalne mode. System broadcasts are sent to all apps that are subscribed to receive the event. 

The broadcast message itself is wrapped in an  `Intent` object whose action string identifies the event that occurred. The intent may also include additional infomration bundled into its sextra field. For example, the airplane mode intent includes a boolean extra that indicates whether or not Airplane mode is on.

For a complete list of system broadcast actions see the `BROADCATS_ACTIONS.TXT` file in the Android SDK. Each broadcast action has a constant field associated with it. For example the value of the constant `ACTION_AIRPLANE_MODE_CHANGED` is android.intent.action.AIRLANE_MODE. Documentation for each broadcast action is avaiable in its associated constant field. 

## Change to system broadcast 
As the android platfrom evolves, it periodically changes how system broadcast behave. Keep the following change in mind if your app targetes Android 7. (API 24) or higher, or if it's installed on devices running android 7.0

Android 9
Beginning wiht Android 9 (API 28) The `NETWORK_STATE_CHANGED_ACTION` broadcast doesn't receive information about the user's location or peronsally identifiable data. In addition, if your app is installed ona device running Androi 9 or higher, system broadcasts from Wi-Fi don't contain SSIDs, BSSIDs, connections infomration or sccan results. To get this information call `getConnectionInfo()`

Android 8
The system imposes additional restrictions on manifest-declared receivers. If your app targets Androi 8 or higher you cannot use the manifest to declare a receiver foro most implicit broadcasts(broadcasts that don't target your app specificlly. You can still use a content register receiver when the user is actively using your app. 

Android 7
Don't send the folowing system broadcast 
- ACTION_NEW_PICTURE
- ACTION_NEW_VIDEO

Also, apps targeting Androdi 7 and higher must register the CONNECTIVITY_ACTION broadcast using registerReceiver(BroadcastReceiver, IntentFilter). Declaring a receiver in teh manifest doesnt' work 


## Receiving broadcasts
Apps can receive broadcast in two ways through manifeest-declared receivers and context registered receivers. 

## Manifest-declared recievers
If you declare a broadcast receiver in your manifest, the systme launches your app (if the app is not already running) when the broadcast is sent. 

To declare a broadcast receiver in the manifest, peform the following

1. Speicry the <reciever> elemetn in your app's manifest
```
<receiver android:name=".MyBroadcastReceiver"  android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="android.intent.action.INPUT_METHOD_CHANGED" />
    </intent-filter>
</receiver>
```

The intent filters spcecify the broacast actions your receiver subscibes to. 

2. Subclass Broadcast reciever and implement onRecieve(context, Intent) the broadcast receiver in the following example logs and dispalys the contents of the broadcast
```
private const val TAG = "MyBroadcastReceiver"

class MyBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        StringBuilder().apply {
            append("Action: ${intent.action}\n")
            append("URI: ${intent.toUri(Intent.URI_INTENT_SCHEME)}\n")
            toString().also { log ->
                Log.d(TAG, log)
                Toast.makeText(context, log, Toast.LENGTH_LONG).show()
            }
        }
    }
}
```

The system package manager registers teh receiver when the app is installed. The receiver then becomes a separate entry point into your app which means that the system can start the app and deliver the broadcast if the app is not curretnly running. 

The system creates a new BroadcastRecier compoennt object to handle each braodcast that it receives. This object is valid only for the duration of the call to onReceive(Context, INtent). Once your code returns from this method, the system considers the comoponent no longer active. 

## Context-registered receivers
To register a receiver with acontext, perfom the following steps. 

1. Create an isntance of the `BroadcastReceiver`
```
val br: BroadcastReceiver = MyBroadcastReceiver()
```

2. Create an `IntentFilter` and register the receiver by caling `registerReciever(BroadcastReciever, IntentFilter)`
```
val filter = IntentFiler(Connectivitymanager.CONNECTIVITY_ACTION).apply {
  addAction(Intent.ACTION_AIRPLANE_MODE_CHANGED)
}
registerReciever(br, filter)
```

Context-registered receiver receive broadcasts as long as their registering context is valid. For an exmample, if you regsiter within anActivity context, you receive broadcasta s long as the activit is not destoryed. If you register with the application context, you receive broadcasta sa long as the app is runing. 

3. To stop receving broadcasts, call `unregisterReceiver(android.content.BroadcastReciever)`. Be sure to unregister the receiver when you no longer need it or the conext is no longer valid. 

Be mindful of where you register and unrgister the receiver, for exampel, if you regsiter a receiver in onCreate(bBundle) using the ativity's context you should unregister it in `onDestory()` to prevent leaking the receiver out of the activity context. If you register a receiver in `onResume()` you shoul dunregister it in onPause() to prevent registering it multple times (if you don't want to receive broadcast when paused, and this can cut down on unnecessar system overhead). Do not unregister in onSaveInstanceState(Bundle) becuase this isnt' calle dif the user moves back in the history stack. 

## Effects on process state
The state of your braodastREceiver (whether it is running or not ) affects the state of ts containing process, which can in turn affect its likelihood of being killed by the ssytem. For exmaple, when a process executes a receiver (that is , currently running the code in its `onReceive(0 method) it is considred to be a foreground process. The system keeps the process running excep tunder cases of extreme memory pressure. 

Howsever, once your code returns from onReceive(), the broadcastReceiver is no longer active. The receiver's host process beomes only as important as the other app components that are running in it. If that process hosts only a amnifest-declared receiver a(a common case for apps that the user has never or not recently interacted with), then upon returning from onReciever() the system ocnsiders its process to be a ow-priority process and may kill it to make resources avaible for othe rmore important processes. 

For this reason,you should not start long running background threas from t a broadcast receiver After onReceiver() the system can kill the process at any time to reclaim memory,a nd in doing so it terminate the spawned thread runningin the process. To avoid this, you should either call goAsync()  if you want a little mor time to process the boradast in ta backgroun thread or schedule a jobService form the receiver using the Jobscheuder, so the system konwns that the proces scontineus to perfom active work. Fo

The following snippet shows a Broadcast reciever tha tuses goAsync() to flag that it needs more time to finsish after onReceive() is complete. This espically usful if the work you want to complete in your onReceive() is long enought cause the UI thread to miss a frame(>16ms) making it better suited for a background thread. 
```
private const val TAG = "MyBroadcastReceiver"

class MyBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        val pendingResult: PendingResult = goAsync()
        val asyncTask = Task(pendingResult, intent)
        asyncTask.execute()
    }

    private class Task(
            private val pendingResult: PendingResult,
            private val intent: Intent
    ) : AsyncTask<String, Int, String>() {

        override fun doInBackground(vararg params: String?): String {
            val sb = StringBuilder()
            sb.append("Action: ${intent.action}\n")
            sb.append("URI: ${intent.toUri(Intent.URI_INTENT_SCHEME)}\n")
            return toString().also { log ->
                Log.d(TAG, log)
            }
        }

        override fun onPostExecute(result: String?) {
            super.onPostExecute(result)
            // Must call finish() so the BroadcastReceiver can be recycled.
            pendingResult.finish()
        }
    }
}

```
## Sending broadcasts
Android provides three ways for apps to send broadcasts
- The sendOrderedBroadast(Intent, String) method sends broadcast to one receiver at a tiem. As each receiver eexecute in turn , it can propagate a result to the next receiver or it can completely abort the boradcast so that it won't be passed to other receivers The order receivers run in can be controlled with the android:priority attribute of the matching intent-filter receivers with the same priororty will be run in an arbitary order

- The sendBroadcast(itent) sends broadcasts to all recievers inan undefined order This called a Normal Broadcast. This more efficient, but means that receivers cannot read results from th other receivers, propagate data received from the broadcast, or abort the broadast
- The localBroadcastmanager.sendBroadast methdo sends broadcasts  acrosss apps, use local broadcasts. The implementations si muc  more efficient (no interprocess communciation needed) and you don't need to worry about any security issues related to other apps being able to receive or send your broadcast. 

The following doe snipptet demonstrates how to send a broadcast by creatinga n intent and caliing sendBroadcast(inent)
```
Intent().also { intent -> 
  intent.setAction("com.example.broadcast.MY_NOTIFICATION"
  intent.putExtra("data", "notice me ")
  sendbroadcast(intent)
}
```
The broadast message is wrapped in an intent object. The intent's action strin gmust provide the apps Java package name syntx and uniquely identify the broadcast event. You can attach additonal information to the intent iwth putExtra(stirng, Bundle). You can salso limit a broadcast to set a of apps in same organization by calling setPackage(Stirng) on the intent. 

Note Although intents are used for both sending broadasts and starting activites with startActivit(intent), these actions are completely unrleated. Broadcast receivers can't see or capture intents used to start an activity; likewise, when you broadcast an intent you can't find or start an activity. 

## Restricitng broadcasts with permissions
Permissions allow you to restrict broadcasts to the set of apps that hold certain permissions. you canenforce restrictions on either th esender or reeiver of a broadcast. 

Sending with permissions
When you call sendBroadcast(intent, string) or sendOrderedBroadcast(intent, String, BroadcastReceiver, Hnadler, int, String, Bundle, you can specify a permission parameter. Only receivers who have requested that permission with the tag in their manifest and suseqently been granted the permission if it is dangerous) can receive the broadcast. For example, the following code sends a boradcast
```
sendBroadcast(Intent("com.example.NOTIFY"), Manifest.permission.SEND_SMS)

To receive the broadcast, the receiving app must request the permission as shown below. 
```
<uses-pemission android:name="android.permission.SEND_SMS/>

You can specify either an existing system permission like SEND_SMS or define a custom permission with the <permission> element. For information on permissons and security in general, see the Syste Permissoins. 

Note: Custom permissions are registered when the app is installed. Th eapp that defines the custom permission musts be installed berfore the app that uses. it. 

## Receiving with permissions 
If you speicfy a pmermission parameter when registering a broadcast receiver (either with registerReceiver(BoracastReceiver, IntentFilter, Stirng, Handler) or in <receiver> tag in your manifest), then only broadcasters who have requested the permission with the<uses-permission> tag in their manifest (and subseuqnetly been granted the permission if iti sdangerous can send an intent to the receiver. 

For example, assume your receving ap phas a manifest-declared receiver as shown bleow
```
<receiver android: name =".MyBroadcast Reciever"
  android:permission ="android.permission.SEND_SMS">
  <intent-filter>
    <action android::name="android.intent.action.AIRPLANEMODE"/>
  </intent-filter>
</receiver>

Or your receving app has a context-registered receiver as shown below. 

```
val filter = IntentFilter(Intent.ACTION_AIRPLANE_MODE_CHANGED)
registerReceiver(receiver, filter, Manifest.permission.SEND_SMS, null)
```

Then to be able to send broadcssts to thosse receivers, the sending app must request the mpermission as shown below. 
```
<uses-permission android:name="ndroid.permission.SEND_SMS"/>
```

## Secuirty considerations and best practices
- If you don't need to send broadcast tocomponents outside of your app, then send and receive local broadcasts with the LocalBroadcastManager which is available in the Support library. The localGroadcastManager is much more  efficient (no interprocess communication needed) and allows you to avoid thingking about any security issues related to other apps being able to receive or send your broadcasts. Local Broadcasts can be used as general purpose pub/sub event bus in your app without any overheads of  system wide broadcast.

- If many apps have registered to receive the same broadcast in their manifest, it can cause th esystem to launch a lot of apps, causing a substantial impact on both device perfomrance and user experience. To avoid this, prefer using context rgistration over manifest declaration.  Sometimes, the android system itself enforces the use of context-registered receivers. Fo example, the CONNECTIVITY_ACTION broadast is delivered only to context-registered receivers. 
- Do not broadcast sensitive infromation using an implicit intent. Th infomration can be read by any app that registers to reeive the broacast. There are three ways to control who can recieve your broadcasts
  - You can specify a permission when sending a broadcast
  - In Android 4.0 and higher you can specify a package with setPackage SS(tring) when sendign a boracast. Thesystem restirct the broadcast to the seto fo apps that match the  package. 
  - You can send local boradcast with LocalBroadcastManger.
- When you register a receiver, any app can send potenially malicious broadcasts to your app's receiver. There are three ways to limit the boradcasts that your app receives
  - You can specify permission when regsitering a broadcast receiver. 
  - For manifest-declared receivers, you can set the adnrodi:exported attribute to "false" in the manifest. The receiver does not receive broadcast form sources outside of the app. 
  -  YOu can limit yourself to only local broadcasts with LocalBroadcastMagner. 
- The name space for broadcast actions is global. Make sure that action names and other strings are written in a namespace you won, or else you may inadvertenly conflict with other apps. 
- Becuase a recevier's onReceive(context, INtent) method runs on the main thrad, it should execute and reutrn quickly. If you ened to perfrom long running work, be careful about spawinging threads or starting backgrouns ervice sbecuase the system can skill the entire processs after onReceive() retursn For more information see effects on process state To perform long running work, we recommend
  - calling goAsync() in your receiver's onReceive() method and passing the BroadcastReciver.PendignResult to a background thread. This keeps the broadcast active after rtuernign from onrEcieve() . However, even with this apprach the system expects you to finish with the broacast very quickly (under 10 seconds) it does allow you to move work to another thrad to avoid glitching the main thread. 
  - Scheduling a job with the JobScheduler Form more information see intenlliget Job scheudling 
- Do not start activites form boradcast receivers becuase the user experience is jarring especially if there i more than on receiver Instead consider displayign a notification

```
````
```
