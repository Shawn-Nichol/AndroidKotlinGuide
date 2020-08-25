# Running a service in teh foreground
A foreground service is a service that the user is actively aware of and isn't a candidate for the system to kill when low on memory. A foreground service must provide a notification for the status bar, which is placed under the ongoing heading. This means that the notification cannot be dismissed unless the service is either stopped or removed from the foreground

For example a music player that plays music from a service should be set to run in the foreground, becuase the user is explicilty aware of its operation. The notification in teh status bar might indicate the current song and allow the user to launch the activity to interact with the music player. Similarly an app to let users track their runs would need a foreground service to track the user's location.

```
val pendingIntent: PendingIntent = 
  Intent(this, ExampleActivity::class.java).let { notificationInent -> 
    PendingIntent.getActivity(this, 0, notificationInent, 0)
  }
  
val notification: Notification = Notification.Builder(this, CHANNEL_DEFAULT_IMPORTANCE)
  .seContentTitle("My Title")
  .setContentText("This the text for the nofication")
  .setSmallIcon(R.drawable.myIcon)
  .setContentIntent(pendingIntent)
  .setTicker("Ticker text")
  .build()
```

To remove the service fromt he foreground, call stopForeGround(). THis method takes a booealn, which indicates whether to remove the status bar notification as well. This method does not stop the services. However, if you stop the service while it's still running in the foreground, the notification is also removed. 
