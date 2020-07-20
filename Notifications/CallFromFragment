Call from a fragment

- Get the Activity context from onAttach()
- In a function call createNotificationchannel
- Obtain NotificationManagaer. 
- run the notification function. 

fun MyFragment : Fragment() {
  
  lateInit var mContext: 
  
  override fun onAttach(context: Context) {
    super.onAttach(context)
    mContext = context
  }
  
  fun runNotification() {
    createNotifcationChannel()
    
    val notificationManager = ContextCompat.getSystemService(mContext, NotificationManager::class.java) as NotificationManager
    
    // Call NotificationUtil an dpass the argments for message and context
    notificationManager.sendNotification("My message", mContext)
  }
  
  function NotificationChannel() {
    // See createNotificationChannel for details
  }
  
}

```
Notification Util
```
fun NotificationManager.sendNotification(message:String, mContext: Context) {
  // See create NotificationUtils for more details. 
}
```
