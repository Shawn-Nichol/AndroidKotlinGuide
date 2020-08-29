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
  setOnlyAlertOnce(true)
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

## System-Wide category
Anddroid uses pre-defined system-wide categories to determine whether to distrub the user with a given notification when the user has enbalbed Do Not Distrub mode. If the notification falls into one of the pre-defined notification categores defined in NotficiationCompat (CATEGORY_ALARM, CATEGORY_REMINDER, CTEGORY_EVENT, CATEGORY_CALL) you should declare it as such by passing the appropriate category to setCategory(). This information is used by the system to determine how to display the notification during Do Not Distrurb mode. 

```
bar builder = NotificationCompat.Builder(this, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.myIcon)
  setContextTitle("My Notification")
  setContextText("This is my notification")
  setPriority(NotificationCompat.PRIORITY_DEFAULT)
  setCategory(NotificationCompat.CATEGORY_MESSAGE)
}
```

## Urgent message
The app might need to display an urgent, time-sensitive message, such as an incomign phone call or a ringing alarm. In these situations, you can associate a full-screen intent with your notification. When the notification is invoked users will see one of the following. 
- Locked Device: a full screen activity appears, covering the lockscreen. 
- Unlocked Device: the notification appears in an expanded form that includes optionis for handling or dismisising the notification. 

```
val fullScreenIntent = Intent(this, MyAcitivty::class.java)
val fullSreenPendingIntent = PendingINtent.getActivity(this, 0, fullScreenIntent, PendingIntent.FLAG_UPDATE_CURRENT)

var builder = NotificationCompat.Builder(this, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.myIcon)
  setContextTitle("My Notification")
  setContextText("This is my notification")
  setPriority(NotificationCompat.PRIORITY_DEFAULT)
  setFullScreenIntent(fullScreenPendingIntent, true)
}
```

## Set Lock screen visiblility
To Control the level of detail viisble in the notification from the lock screen, call setVisibility() and specify one of the following
- VISIBILITY_PUBLIC: Shows the notification's full content
- VISBILITY_SECERT: doesn't show any part of the notification on the lock screeen
- VISIBILITY_PRIVATE: Shows basic information such as the notification's icon and the content title, but hides the notificaitons full content. 

When VISIBILITY_PRIVATE is set, you can also provide an alternate version of the notification content which hids cetain details. For example an SMS app might display a nofitication that shows you have 3 new text messages, but hides the message contents and senders. To provide this alternative notification, first create the alternative notification with NotificationCompat.Builder as usual. Then attach teh alternative notification to the normal notification setPublicVersion() 

The user still has final control over whether their notifications  are visible on the lock screen. 

## Remove a notification
Notification sremain visible until on of the following happens
- The user dismisses the notification
- The user clicks the notification, and setAutoCancel() was applied to the noification. 
- Cancel(): for a specific notification ID. THis mehtod also deletes ongoing notification
- cancelAll(): removes all of the notification you previously issued.
- If you set a timeout when creatinga  notification using setTimeoutAFter(), the system cancels the notification after the specified duration elapses. If requried, you can cancel a notification before the specified timout duration elapses. 


# Note
The Android system will remember the prioty level set by the user, if the user has set a low prioty and you are adjusting the code for a higher priority this may result in the notification not working as inteneded. At this point it is best to uninstall the app and reinstall it. 
