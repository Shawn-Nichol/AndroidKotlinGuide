Create a Notifction Util file

- Create a fun to send the message
- Create a Notification builder, the builder requires a small Icon, ContentTitle, Content Text
- set the priority of the builder. 
- show the notifiation

```
fun NotificationManager.sendNotification(message: String, mContext: Context) {
  
  // Create an explicit intent for an Activity(specify what activity you want to run)
  val contentIntent = Intent(mContext, MainActivity::clasas.java
  
  /*
   * PendingIntent lets us pass a future intent to another application, which allows that application to execute
   * that intent as if it had the same permissions as our application.
   *
   * - Context: this is the context in which the PendingIntent starts teh activity. 
   * - Requestcode: This assigns a number to the PendingIntent so this PendingIntent can be used later on. 
   * - Intent: Explicit intent object of the activity to be launched. 
   * - flag: check link for available flags do https://developer.android.com/reference/android/app/PendingIntent#FLAG_CANCEL_CURRENT
   */
   val contentPendingIntent: PendingINtent! = PendingIntent.getActivity(
    mContext,
    REQUEST_CODE,
    contentIntnet,
    PendingIntent.FLAG_ONE_SHOT
    )
  
  // Convet a png file to a Bitmap for the large Notification Icon.
  var largeIcon = BitmapFactory.decodeResource(mContext.resources, R.drawable.mypng)
  
  /*
   * Build the noitication
   * Constructor requires context, CHANNEL_ID
   * setSmallIcon: displays the icon in the status bar
   * setContentTitle: sets the Title of the notification
   * setContentText: set the text in the notification.
   * SetLargeIcon: Displays a large icon in the notification whem minimized on the right side. 
   * SetStyle, BigrPicture: Displays a large image when the Notification is expanded and a small icon when it isn't. 
   * setPriority: Determines how intrusive the notificatioin is. 
   * setContentIntent: sets the Intent that will fire when the user touches the notificaion.
   * setAction: will display a button with text, you can have up to three
   * setAutoCancel: will automatically close the notification after the user taps it. 
   */
   val builder = NotificationCompat.Builder(mContext, message) 
    .setSmallIcon(R.drawable.notification_icon)
    .setContentTitle("My Notification")
    .setContenetText("This is what my notification is going to read")
    .setLargeIcon(largeIcon)
    .setStyle(NotificationCompat.BigPictureStyle() 
      .bigPicture(largeIcon)
      .largeIcon(null))
    .setPriority(NotificationCompat.PRIORITY_HIGH)
    .setContentIntent(contentPendingIntent)
    .setAction(R.drawable.imageOne, "Btn One", ContentIntent)
    .setAction(R.drawable.imageTwo, "Btn Two", ContentIntent)
    .setAutoCancel(true)
    
  /*
   * Show the notification
   * with: calls teh specified function block with as its reciever and returns its results.
   * NotificationManagerCompat: Notify the user something has happened, compat is backwards compatible. 
   * from: instance of provided context.
   * notify: post a notification to be displayed in the status bar.
   */
   with(NotificationManagerCompat.from(mcontext)) {
    notify(NOTIFICATION_ID, builder.build())
   }
   
}
```
