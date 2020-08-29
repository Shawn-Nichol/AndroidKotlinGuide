# Create a Notification

## Createa a channel
Before a notification is deliveried a notification channel needs to be registered. You should run this when the activity starts, it's safe to call repeatedly becuase creating an existing notification channel perfrom no operatioin. 
```
private fun createNotificationChannel()( {
  // Create the NotificationChannel if the API is higher than 26+
  if(Build.VERSION.SDK_INT >= Build.VERSION.CODES.O) {
    val name = "My Service Channel"
    val desriptionText = "This is my notification, it used to inform the user of."
    val importance = NotificationManager.IMPORTANCE_DEFAULT
    val channel = Notification(CHANNEL_ID,  name, importance).apply {
      description = descriptionText
    }
    
    // Register the channel with the system
    val nvm: NotificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
    notifcationManager.createNotificationChannel(channel)
  }
}
```


## set Notification content
To create a notification you need to create a notification object with a context and channel ID argument. 
```
var builder = NotificationCompat.Builder(this, CHANNEL_ID)
  .setSmallIcon(R.drawable.myDrawable)
  .setContentTitle("Notification Title")
  .setContentText("Notification info")
  
```

.setStyle will allow you to modify the notification


## set Tap action
Every noitification should respond to a tap, create a pending intent for the setContextIntent argument. 
```
val intnet = Intent(this, MyActivity::class.java)
val pendingIntent = PendingIntent.getActivity(this, 0, intent, 0)

val builder = NotificationCompat.Builder(this, CHANNEL_ID)
  .setSmallIcon(R.drawable.myDrawable)
  .setConextTitle("Notification Title")
  .setContextText("This is my notification")
  .setContextIntent(pendingIntent)
  .setAutoCancel(true) // Removes the notifcation after tap.

```

## Action Button
A notification can have up to three action buttons, the buttons should not duplicate the action of taping the notification. To add an action button pass a PendingIntent to the addAction() method. This is just like setting up the notification's default tap action except instead of starting an activity, you can doa variety of other things such as start a BroadcastReceiver that performs a job in the background so the action does not interrupt the app that's already open. 

```
val snoozeIntent = Intent(this, MybroadcastReceiver::class.java).apply {
  action = ACTION_SNOOZE
  putExtra(EXTRA_NOTIFICATION_ID, 0)
}

val snoozePendingIntent: PendingIntent = PendingIntent.getBroadcast(this, 0, snoozeIntent, 0)
val builder = NotificationnCompat.Builder(this, CHANNEL_ID)
  .setSmallIcon(R.drawable.myDrawable)
  .setContextTitle("My title")
  .setContextText("This is my notification")
  .setPriority(NotifiactionCompat.PRIORITY_DEFAULT)
  .setContentIntetn(pendingIntent)
  .addAction(R.drawable.myAction, "Action", snoozePendingIntent)
```

## Progress Bar
Notification can include an animted progres indicator that sows users the status of an ongoing operation. If you can estimate how much of the operation is complete at any time, use the "determinate" form of the indicator by calling setProgress(max, progress, false). The first parameter is the complete value, the second value is how much is currently complete and the alst indicates this is a determinate progress bar. As the operation proceeds, continuously call setProgress(max, progress, false) with an updated value ofr progress and re-issue the notification. 

```
val builder = NotificationCompat.Builder(this, CHANNEL_ID).apply {
  setContentTitle("Picture download")
  setContentText("Downlaod in progress")
  setSmallIcon(R.drawable.myDrawing)
}
val PROGRESS_MAX = 100
val PROGRESS_CURRRENT = 0
NotificationManagerCompat.from(this).apply {
  // issue the initial notification with zero progress
  builder.setProgress(PROGRESS_MAX, PROGRESS_CURRENT, false)
  notify(notifcationID, builder.build())
  
// Show progress update, and update the notificatino with builder. 
builder.setProgress(PROGRESS_MAX, PROGRESS_CURRENT, FALSE);
nvm.notify(notificationId, builder.build())

// When the progress bar is complete, call it one last time to remove the progress bar
builder.setContentText("Download Complete")
  .setProgress(0,0,false)
notify(notificationId, builder.build())
}
```

To set a continus progress bar setProgress(0,0, true)
