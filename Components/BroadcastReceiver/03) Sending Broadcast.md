# Sending Broadcast
Android provides three ways to send a broadcast

## 01) SendOrderBroadcast
The sendOrderedBroadcast(Intent, String) methods sends broadcasts to one receiver at a times. As each receiver executes in turn it can propagate a result to the next receiver, or it can completely abort the broadcast so that it won't be passed to other receivers. The order receivers run in can be controlled with the android: priority attribute of the matching intent-filter, receivers with the same priority will be run in an arbitrary order. 

## 2) SendBroadcast
The SendBroadcast(Intent method sends broadcasts to all receivers in an undefined order. This is called a normal Broadcast. This more efficient, but means that receivers cannot read results from other receivers, propagate data received from the broadcast, or abort the broadcast. 

## 3) LocalBroadcastManager
The LocalBroadcastManager.sendBroadcast() method sends broadcast to receivers that are in the same app as the sender. If you don't need to send broadcasts across apps, use local broadcast. The implementation is much more efficient (no Interprocess Communication needed) and you don't  need to worry about any security issues related to other apps being able to receive or send the broadcast. 

```
Intent.also { intent -> 
  intent.setAction("com.example.broadcast.MY_NOTIFICATION")
  intent.putExtra("data", notice me")
  sendBroadcast(intent)
```
The broadcast message is wrapped in an Intent object. THe intent's action string must provided the app's Java package name sysntax and unique idtenty for the broadcast event. You can attach additional information to the intent with putExtra. You can also limit a broadcast to a set of apps in the same organization by calling setPackage(string).


# Sending permissins
When you call sendBroadcast(Intent, String) or sendOrderedBroadcast(Intent, String, BroadcastREceiver, Handler, Int, String, Bundle), you can specify a permission parameter. Only receivers who have requested that permission with the tag in their manifest (and subsequently been granted the permission if it is dangerous) can recieve the broadcast. For example, the following code sends a brodcast. 
```
sendBroadcast(Intent("com.example.NOTIFY"), Manifest.permissionSEND_SMS)
```

To recieve the broadcast, the receiving app must request the permission as shown blew.
```
<uses-permission android:name="android.permission.SEND_SMS"/>
```

You can specify either an existing system permission like SEND_SMS or define a custom permission with the element. For information on permissions and security in general, see the System Permissions

Note; Current permissionare registered when the app is installed. The app that defines the custom permission bust be installed beofre the app that uses it. 


