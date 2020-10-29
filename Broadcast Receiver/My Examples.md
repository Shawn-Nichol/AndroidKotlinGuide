Manifest-delcared recievers are now depracted so there will be no example on them. 

# Create Context-Registered Receiver

## 1) Create the Broadcast Class
Create a broadcast class and override onRecieve(), 
- onReceive will run when the Broadcast class receives an event. 
```
class MyBroadcastReceiver : BroadcastReceiver() {
  override fun onReceive(context: Context, intent: Intent) {
    StringBuilder().apply {
      append("Action: ${intent.action}/n")
      toString().also { log ->
        Log.i("MYTESTa Broadcast", log)
        Toast.makeText(context, log, Toast.LENGTH_SHORT).show()
  }
}
```

## 2) Create an instance of the BroadcastReceiver
```
val br: BroadcastReceiver = MyBroadcastReceiver()
```

## 3) register the Receiver
Create an IntentFilter and register the receiver, run in the MainActivity or where ever it is needed. 
```
val filter = IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION).apply {
  addAction(Intent.ACTION_AIRPLANE_MODE_CHANGE)
}

registerReciver(br, filter)
```
