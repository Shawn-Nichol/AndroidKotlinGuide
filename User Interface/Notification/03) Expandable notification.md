# Expandable notfication
Build a notification with all the basic content. Then call `setStyle()` with a style object and supply information corresponding to each template.

Large Image
```
var notification = Notification.Builder(context, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.new_post)
  setContentTilte(imageTitle)
  setContentText(imageDescription)
  setStyle(NotificationCompat.BigPictureStyle().bigPicture(myBitmap))
  build()
  
}
```

To make the image appear as a thumbnail only whil ethe notification is collapsed call setLargeIcon() and pass it the image, but also call BigPictureStyle.bigLargeIcon() and pass it null so the large icon goes away when the notifcation is expanded.
```
var notification = NotificationCompat.Builder(context, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.new_post)
  setContentTitle(imageTitle)
  setContentText(imageDescription)
  setLargeIcon(myBitmap)
  setStyle(NotificationCompate.BigPictureStyle()
    .bigPicture(myBitmap)
    .bigLargeIcon(null))
  build()
}
```

Add a large block of text
```
var notification = NotificationCompat.Builder(context, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.new_mail)
  setContentTitle(emalObject.getSnederName())
  setContentText(emailObject.getSubject())
  setLargeIcon(emailObject.getSenderAvatar())
  setStyle(NotificationCompat.BigTextStyle()
    .bigText(emailObject.getSubjectAndSnippet()))
  build()
}
```

## Create an inbox-style notification
Apply NotificationCompat.InboxStyle to a notification if you want to add multiple short summary lines, such as snippets from incoming emails. This allows you to add multiple pieces of content text that are each truncated to one line, instead of one continuous line of text provided by NotificationCOmpat.BigTextStyle

```
var notification = NotificationCompat.Builder(context, CHANNEL_ID).apply {
  setSmallIcon(R.drawable.new_mail)
  setContentTitle("5 New mails from " + sender.toString())
  setContentText(subject)
  setLargeIcon(aBitmap)
  setStyle(NotificationCompat.InboxStyle()
    .addLine(messageSnippet1)
    .addLine(messageSnippet2))
}
```


## Show a conversation in a notification
Apply NotificationCompat.MessagingStyle to dispaly seqential messages between any number of people. THis is ideal for messaging apps because it provides a consisten tlayout for each message by handling the sender name and message text separately, and each message can be multiple lines long. 

TO add a new message, call addMessage(), passing the message text, the time receiveed, and the sender name. You can also pass this infomration as a NotificationCompate.MessaginingStyle.Message object.
```
var message1 = NotficationCompat.MessagingStyle.Message(messages[0].getText(),
  messages[0].getTime(),
  messages[0].getSender())
var message2 = NotificationCompat.MessagingStyleMessage(messages[1].getText(),
  messages[1].getTimer(),
  messages[1].getSender())
var notification = NotificationCompat.Builder(context, CHANNEL_ID)
  .setSmallIcon(R.drawable.new_message)
  .setStyle(NotificationCompate.MessaginingSTyle("Reply with name")
    .addMessage(message1)
    .addMessage(message2)
  .build()
```
