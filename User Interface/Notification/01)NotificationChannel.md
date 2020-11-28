## Notification Channel
After you create a Notification channel, you cannot change the notification behaviors, the user has complete control at that point. Through you can still change a channels namd and desricption. You should create a channel for each distinct type of notification you need to send. You can slo create notification channels to reflect choices made by users of your app. 


## Createa a channel

1. Construct NotificationChannel object with a unique ID, a user visible name and an importance level. 
2. Optionally, specify the description that the user sees in the system setting with setDescription()
3. Register the notification channel by passing it to createNotificationChannel(). 

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

IMPORTANCE level
- Urgent
Makes a sound and appears as a heads-up notification

- High
Makes a sound

- Medium
No sound

- Low
No soudn and does not appear in the status bar. 

All notification,regardless of importance, appearn in non-interruptive system UI loctions, such as in the notification drawer and as a badge on teh launcher icon, though ou can modify the appearnce of the notification badge. 

Once you submit the channel ot the Notificationmanager, you cannot change the importance level. However the user can chagne their preferences for your app's channel at any time. 
