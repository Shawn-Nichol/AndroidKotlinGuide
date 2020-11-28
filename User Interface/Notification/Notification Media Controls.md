## Create a Notification with Media Controls
Apply NotificationCompat.MediaStyle to display media playback controls and track information.

Call `addAction()` up to five times to display up oto five separate icon buttons. And call `setLargeIcon()` to set the album work. 

Unlike th eother notification styles, MediaStyle allows you to also modify the collapsed-size content view by specifying three action buttons that should also appear in the collapsed view. To do so, provided the action button indices to `setShowActioninCompatView()`.

In the notfication represents an active media sesssion, also attach a `MediaSession.Token` to the notfication using `setMediaSession(). Android then identifies this as a notification representing an active media session and respond accrodingly . 

```
var notfication = NotificationCompat.Builder(context, CHANNEL_ID).apply {
  // show controls on lock screen even when the user hides sensitive content. 
  setVisiblity(Notification.VISIBILTY_PUBLIC
  setSmallIcon(R.drawable.ic_stat_player)
  // Add media control buttons that invoke intents in your media service
  addAction(R.drawable.ic_prev, "Previous", prevPendingIntent)
  addAction(R.drawable.ic_pause, "Pause", pausePendingIntent)
  addAction(R.drawable.ic_next, "Next", nextPendingIntent)
  // Apply the media style
  setStyle(MeidaNotficationCompat.MediaStyle()
    .setShowActionsInCompatViw(1 /* # 1: pause button \*/)
    .setMediaSession(mediaSession.getSessionToken()))
  setContentTitle("Wonderul music")
  setContentext("My awesome band")
  setLargeIcon(albumArtBitmap)
  build()
}
```
