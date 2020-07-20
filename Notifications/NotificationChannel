Create a Notification channel

A notification channel repesents a number of settings that apply to a collection of similarly themed notifications. Starting in API 26+ all notifictions must be assigned to a 
channel. After you create a notification channel, you cannot change the notification behaviors, the user has complete control of the behaviors at this point. 

- create a check notification function
- Check the API level of the of the app
- The constructor for the NotificationChannel requires a Unique channel ID, a user visible name, and a importance level
- Specify the description that he user sees(Optional)
- Register the notification Channel by passing it to creatNotification


```
fun CreateNotificationChannel() {
  if(Build.VERSION.SDK_INT >= Build.VERSION_CODE.O) {
    // Channel name
    val name = "MY Ch Notification"

    // Channel description
    val descrtiptionText = "This is my notification channel, it will alert the  user of a change.

    // Importance: This parameter determines how to interup the user for any notification that uses this channel. importance uses a constant from the NotificationManager. 
    val importance = NotificationManager.IMPORTANCE_DEFAULT

    /*
     * Create notification channel, 
     * CHANNEL_ID: is an int that should be stored in the Notifcation Util file, can be any number you want. 
     * apply calls the spedified function block with 'this' value as its receiver and returns 'this value.
     */
    val channel: NotificationChannel = NotificationChannel(CHANNEL_ID, name, importance).apply {
      description = descriptionText
    }

    /*
     * Register the channel with the syste. 
     * requireActivity: invokes getActivity, and returns null if the fragment isn't attched to an activity. 
     * getSystemService: Get the gontext from. 
     * NotificationManager: NotificationManager is how you notify the user that something has happened in the background. 
     * :: Reflection: is a set of language and library gestures that allows for introspecting the structure of your own program at runtime. 
    val notificationManager: Notification? = requireActivit().getSystemService(NotificationManager::class.java)

    // CreateNotificationChannel:  Creates a notification channel that notification can be posted to. 
    notificationManager.createNotificationChannel(channel)
  }
}
```

It is recommend to run the create Notification function once the app start but it could be run when ever and run multiple times.  
