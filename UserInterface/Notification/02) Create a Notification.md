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
