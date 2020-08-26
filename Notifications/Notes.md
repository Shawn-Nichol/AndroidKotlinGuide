# Overview

A notfication is a message taht android displays outside your app's UI to privde the user with reminders, communication from other people, or other timely information from your app. Users can  tpa the nontification to open our app or take an action directly from teh notfication 

## Appearances on a device
Notficaionts appear to users in differnet locations and romats such as an icon inteh status bar, a more entry in the notfication drawer, as ab adge on the app's icona nd on paired wearrables autmatically

## Status bar
When you issua a notification, it first appears as an icon in the status bar

Uses can swipe down on the status bar to open the notification drawer, where they can veiew more details and  take actions with the notification. 

drawer
Users can swipe down on teh status bar to open the notification drawer, where they can view more details and take actions with the notification. Users can draw down oon a notfication in teh drawer to reveal the expanded view, which shows additional content and action buttons, if provided. Anotficaiont remains visible in the notifcation drawer until dismissed by teh app or the user. 

Heads-up notficiation
Starting in Android 55 notifications ca breifly appear in a floating window called a heads-up notifcation. This behavior is normally for important notifcation that the user should know about imddeciately and it appears only if teh device is unlocked. The headsup notification  appears the moment your app issues the notfication and it disappears after a momemtn, but remains visible in teh notification drawer as usual. 

## Lock screen

You can programmatically set the level o fdetail visible in notification posted by your app on a secure lock screen, or even whether the notfication will show on the lock screen at all. Users can use the system settings to choose the level of detail svisible in lock screen notifications, including the option to disable all lock screen notifications. Starting wit h android 8.0 users can choose to disable or enable lock screen notifications for each notification channel. 

## APp icon badge

In supporete launchers on devices running android 8.0 and higher, app icons indicate new notificcatioons with a coloreed badge also known as notification dot

users can long-press on an app icon to see teh notifcations for that app. users can then dismiss or act on notifcations from that menu, similar to the notifcation drawer. 

## Notification Actions
Although its not requried, every notification should open an appropriate app activity when tapped. In addition to this default notification action, you can add action buttons that complete an app-related task from the notification (often without opending an activity). You can also add an action to reply to messages or enter other text directly from the notification. Starting in Android 10 the platform can automatically generate action buttons with suggested intent-based actions. 

## Expandable notificaion 
By default the notificaiont text content is truncated to fit on one line, If youwant your notificaion to be longer, you canebable a larger text area that's expandable by applying an additional tempalte. you can also create an expandable notification with an image, in inbox style, a chat conversation, or media playback ontrols. For more information, read Create an Expandable Notification

And although we recommend you always use thesse templates to ensure proper design compatibility on all devices, if necessary, you can also create a custom notification layout. 

## Notification updates and groups. 

To avoid bombarding your users with multiple or redundant notifications when you have additional updates you should consider updating an existing notification rather than issues an new one, or consider using the inbox -style notification to show conversation updates. 

however, if it's necessary to deliver multiple notifications, you should consider grouping those separate notifications in to a group. A notification group allows you to collapse multiple notificatioons into just one post in the notification drawer, with a summary. The user can then expand the notifcaiont ot reveal the details for each individuall notification. 

## Notification Channesl Startin in Android 8 all notification smust be assigned to a channel or it will not appear. By categoriziang notifications into channels users can disable specific notification channels for our app and users can control teh visual nad auditory options for each channel all from teh Android system settings. Users can also long-press a notification to change behaviors for the associated channel. 

One app can have multipel notfication channesl a separate channel fore each type of notification the app issues. An app can also create notification channels in response to choices made by users of your app. For example you may set up separate notification channels for each convsation group created by a user in a messaging app. 

The channel is also where you specify the imporotance level for tyour notification on Android 8 and higher. So all notifcaionts posted to the same notifcaition channel have the same behavior

## Notification Importance
Android uses the importance of a notification to determine how much the notification should interrupt the user. The higher the importance of a notifciation, the more interruptive the notification will be. 

On Android 8 and avoe importance of a notification is determined by teh importance of the channel the notification was posted to. users can change the importance of a notification channel in the system settings and below improtance of each ntoffification is deteremined by the notifications priority. 

Urgen: Makes a sound and appears as a head-up notificaiton
High: Makes a sound
Medium: No sound
Low: No sound and does not appear in teh status bar

All notifications regardless of importance, appear in non-interruptive system UI locations, such as in the notification drawer and as abadge on teh launcher icon


## Do not dstrub mode Do not distrube mode, which silences ounds and vibrations for all notifications. Notifications still appear in teh system UI as normal, unless the user specifies otherwise

There are three different levels available in DO Not Distrurb mode

- Total silence: blocks all sounds and vibraetions, including  from alarms music videos and games
- Alarms only: blocks all sounds and vibrations, except from alarms
Priority only: users can configure which system-wide categoreis can interrupt them( such as only alarms, reminders, events, calls, or messages. For messages and calls, users can also choose to filter based on who the sender or caller is.

On Android 8 and above, users can additionall allow notifications through for app specific categories by overriding Do Not distrub on a channel-by-channel basis.

## Notfiication for foreground services

A notification is required when your app is running a foregroun service a service running in the background tha's long living and noticeable to the user, such as media player. this notification ccannot be dismissed like other notifcations. To remove the notifcation the service must be either stopped or emoved form the foregroun state. 


