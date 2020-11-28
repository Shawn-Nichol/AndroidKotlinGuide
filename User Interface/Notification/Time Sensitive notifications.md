# Display time-sensitive notifications
IN specific situations, your app migh need to get the user's attention urgently, such as an ongoing alarm or incoming call. You migh have previously configured your app for this purpose by launching an activity while your app was in the background. 

## Create a high-priority notification
When creating the notification,  make sure that you include a desriptive title and message. ptionally, you can also provide a full-screen intent. 

An example notifcation appears in teh following code snippet. 

```
val fullScreenIntgent = Intent(this, CallActivity::class.java)
val fullScreenPendingINtent = PendingIntent.getActivity(this, 0, fullScreenIntent, PendingINtent.FLAG_UPDATE_CURRENT)

val notifcationBuilder = Notification.Builder(this, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.my_icon)
  setContentTitle("Incoming call")
  setContentText("(403)555-1234")
  setPriority(Notification.PRIORITY_HIGH)
  setPriority(Notification.CATEGORY_CALL)
  
  // Use a full-screen intent only for the highest-priority alerts where you have an associated activity that you wouldl like to launch after the user
  // interacts with the notification. Also, if your app targets android 10 or higher, you need to request the USE_FULL_SCREEN_INTENT permission in 
  // order for the platform to invoke this notification.
  .setFullScreenIntent(fullScreenPendingIntent, true)
  .build()
}
```

## Display the noticatio nto the user
When displayin your notification to the user, they can then choose to acknowledge or dismiss your app's alert or reminder. If the notification is an ongoing one, such as an iiincoming phone call, associate the notification with a foreground service. The following code snippet shows how to dispaly a notfication that's associated with a foreground service. 

```
// Privde a unique integer for the notficationId of each notification
startForeground(notificationId, notfication)
```
