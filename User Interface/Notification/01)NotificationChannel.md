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
