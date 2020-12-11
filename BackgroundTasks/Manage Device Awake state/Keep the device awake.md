To avoid draingin the battery, an android device that is left idel qucikly falls asleep. Hwever,there are times when an applciation needs to wake up the screen or the CPU and keep it awake to complete some work. 

The appraoch you take depends on the needs of your app. However, a general rul of thumb i s that you should use the most lightwieght apprach possible for your app, to minnimize  your app's impact on system resources. The following sectiosn descibe how to handle the cases wehre the device's default sleep behavior is icompatible withthe reuirements of your app. 

## Alternatives to using wake locks
Before addign wakelock support to your app, consider whether your app's use cases support one of the following alternative solutions
- If your app is performing long-runing hTTP downloads, consider using DownloadManager.
- If your app is synchronizing data from an external server, consider creating a sync adapter. 
- If your app relies on background services, consdider usign JobScheduler or Firebase cloud messaging to trigger these eervices at specific intervals. 

## Keep the screen on
Certain apps need to keep the screen turned on such as games or moves apps. The best way to do this is to use th FLAG_KEEP SCREEN_ON in your activity (and only in activity, never in  as ervice or other app componetn. For exmaple. 

```
class MainActivity : Activity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        window.addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON)
    }
}
```
Using android:keepScreenOn="true" is equilvalent to using FLAG_KEEP_SCREEN_ON. You can use wichever approach is beest for your app. The advantage of setting the flag progammically in your activit is tht it gives you the option of programmicall clearing the flag later an therby allowing the screen to tturn off. 

Note you don't need to clear the FLAG_KEEP_SCREEN_ON flag unless you no longer want to the scren to stay on in your urnning application (for exmaple, if you want the screen to time out after a certain period of inactivity). the window manger takes care of ensuring that the righ thign happens when the app goes into the backgroun or returns to the foregorund. But if you wan to explicitly clear the flag and therby alllow the screen to trturn off again, use clearFlags()

## Keep the CPU on

If you need to keep the CPU running in order tom oplete some work before the device goes to sleep, you can use a PowerManager system service fearture calle wake locks. Wake locks allow your applcition to control the power state of the host device. 

Creating and holding wake locks can have a dramatic impact on the host device's batter life. Thus you should use wake locks only when strictly necesssary and hold them for a shosrt a time as possible For example, you should never need to use a wake lock ina n activity. As descibed above if you want to keep the screen on in your activity, use FLAG_KEEP_SCREEN_ON

One legitimate case for using a wake lock might be a backgrounsservice that needs to grab a wak lock to keep the CPU running to do work wihile the sscreen is off. Again through this practice shoudl be mimimized becuase of its impact on battter life. 

To use a wake lock,the first step is to add the WAKE_LOCK permission to your applications manifest
```
<uses-permission android:name="android.permission.WAKE_LOCK" />
```

If your app includes a broacast receive rthat uses a service to do some work, you canmanage your wake lock through a WakefulBroadcastreeciever, as descbied in Use a broadcast receiver that keeps the device aawake. This is preeferred apprach. If your app doesn't follow that patter, there is how you set a wake lock directly. 

```
val wakeLock: PowerManager.WakeLock =
        (getSystemService(Context.POWER_SERVICE) as PowerManager).run {
            newWakeLock(PowerManager.PARTIAL_WAKE_LOCK, "MyApp::MyWakelockTag").apply {
                acquire()
            }
        }
  
```

To realease the wake lock , call wakelock.release(). This releases your claim to the CPU . it's improtant to release a  wake lock as soon as your app is finsiehd using it to avoid draing the batter. 

## use a broadcast receiver tht keeps the device awake
Using a broadcst recevier in conjunction with a service lets you manage the lifecycle of background task. 

A wakefulBroadcast recevier is special type of broadcast receiver that takes care of creatin and manageing a Partial_WAKE_LOCK fo  your app. A wakeFulBroadcastRecevier passes off the work to a Service (typically na INtentService), while ensurign that the cdevice deos not go back to sleep in the transition. If you don't hld a wake lock while transitionign the work to a service, you are effectively allowing the device to go back to sleep before the work completes. The net result is that th app might not finish doing the work until some arbitarry point in the future,which is not what you want. 

The first step in using a WakeFulbroadast Receiver is to add it to your manifest, as with any other broadcast recevier
```
<receiver android:name=".MyWakefulReceiver"></receiver>
```

The follwoing code starts MyIntentServie with the method startWakefulService(). This method is comparable to startService(), expect that th eWakefulBroadcastReceiver is holding a wake lock when the service starts. The intent that is passed with startWakefulService hodls an extra identifiying the wake lock

```
class MyWakefulReceiver : WakefulBroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {

        // Start the service, keeping the device awake while the service is
        // launching. This is the Intent to deliver to the service.
        Intent(context, MyIntentService::class.java).also { service ->
            WakefulBroadcastReceiver.startWakefulService(context, service)
        }
    }
}
```

When the servie is finsihed it calls MyWakefulReciever.completeWakefullIntetn() to release the ewake lock. The complette WakeFfullIntent(0 method has a s aits parameter the same intent that was passed in from the wakefulBroadcastReceiver
```
const val NOTIFICATION_ID = 1

class MyIntentService : IntentService("MyIntentService") {

    private val notificationManager: NotificationManager? = null
    internal var builder: NotificationCompat.Builder? = null

    override fun onHandleIntent(intent: Intent) {
        val extras: Bundle = intent.extras
        // Do the work that requires your app to keep the CPU running.
        // ...
        // Release the wake lock provided by the WakefulBroadcastReceiver.
        MyWakefulReceiver.completeWakefulIntent(intent)
    }
}
````
