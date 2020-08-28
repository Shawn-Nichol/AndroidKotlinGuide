# What is a Notification  
A notificaiton is a message that Android displays outside of an apps UI to provide the user with reminders, commuiincation from other people, or other timely information from your app. Users can tap the notification to open your app or take an action directly from the notification.

The design of a notification is determined by system templates your app simply defines the contents for eac poortion of the template. Some dettails of the notification appear onlyl in the expanded view. 

- Small Icon: This is required
- Large Icon: This is optional, usually used for contact photos.
- App name: provided by the system
- Time stamp: This provided by the system by you can override with setWhen()
- Title: This is optional
- Text: This optional.

## Notification actions
Alought it isn't required all notification should open an appropriate app when tapped. In addition to this default notification action, you can add action buttons that complete an app-related task fromt he notification 

## Expandable notification
By default notification's text is truncated to fit one line. You can enable a larger text area that's expandable by applying an additional template. 

## Notification updates
To avoid bombaring your users with mutiple or redundant notifications when you have additional updates, you should update an existing notificatino rather than issuing a new one. However if its necessary to deliver multiple notifications, you should consider grouping those separate notificaions into a group. A Notification group allos you to collapse multiple notification inot just one post in the notification drawer, with a summar. The user can then expand the notification to reveal the details of each individual notification

## Notification Channel
All notificaion must be assigned to a channel or it will not appear. By categorizing notifications into a channels, users can disable specific notifcations channesl for your app and users can control the visual and audditory options for each channel. Users can also long press notification to change behaviors for the associated channel. Apps can contain more than one notification channel

## Notification Importance
Android uses the importance of a notification to determine how much the notification should interrupt the user. The higher the importance of a notification, the more interruptive the notification will be. Importance of the notification is determined by the importance of the channel the notifcaion was posted to. users can change the importance of notification channel in the system settings. 

- Urgent: Makes Noise
- High: Makes a sound
- Medium: No sound
- Low: No sound

## Do not Distrub 
Users can enable Do not Distrub mode, which silences ounds and vibration for all notifications. Notifications still appear in teh system UI as normal, unless the user specifies otherwise. 

## Posting limits
Apps cannot make a notification sound more than once per second. If your app posts multiplenotification in one second, they all appear as expected, but only the first notification per second makes the soud. Android applies a rate limit  when updating a notification. If you post updates toa single notification too frequently the sytem might drop some updtes. 

# Appeances on Device
There a number of notification here is brief list

### Status bar and notification drawer: 
When a notification is first issued it appears as an icon in the status bar. Users can swipe the status bar down to get more details of the notification.

### Heads up Notification
Notification can breifly appeaer in a floating window called a heads-up notification. This is for notifications that the user should know about imdediatley, it only appears if the device is unlocked. 

### Lock Srceen 
You can programmatically set the level of detail visible in notifications posted by your app on a secure lock screen, or even whether the notification will show on the lock screen at all. Users can uses system settings to choose the level of details visible in lock screeen notifications, including the option to disable all lock screen notifications. 


### Icon badge (notification dot)
A dot will appear on the app icon when there is a notification. Users can long press the icon to see the notification.

### Wear OS device
All notification will appear on a paired device. 
