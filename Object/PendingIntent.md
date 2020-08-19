# Pending Intent

A PendingIntent object is a wrapper around an Intent object. The primary purpose of a  PendingIntent is to grant permission to a foreign application to use the contained Intent as if it were executed from your app's own process. A PendingIntent itself is simply a reference to a token maintained by the systme descibing the original data used to retrieve it. This means that, even if its owning application's process is killed, the PendingIntent itself will remain usable from other processes that have been given it. If

Major use cases for a pending intent include the following
- Declaring an intent to be executed when the user performs an action with your Notification
- Declaring an intent to be exectued when the user perform an action with your App widget
- Declaring an intent to be executed at a specified future time

Just as each Intent object is designed to be handled by a specific  type of app conponent, os too must a PendingINtent be created with the same consideration. When using a pending intent, your app doesn't execute the intent with a call such as startActivity(). Instead you must declare the intended component type when you create the PendingINtnet by calling the respective creator method: 

- PendingIntent.getActivit() for an Intent that starts an Activity
- PendingIntent.getService for an Intent that starts a service. 
- PendingIntent.getBroadcast() for an intent that starts a Broadcast receiver

unless your app is receiving pending intents from other apps, the above methods to createa a pEndingINtent are probably the only PendingINtent methods you'll everneed. 

Each method takes teh current app Context, the Intent you want to wrap, and one or more flags that specify how the intent should be used

It is important to know when two Intents are considered to be the same for purposes of retrieving a PendingIntent. A common mistake is creating multiple PendingIntentObjects with Intents that only vary in the "extra" contents, expecting to get different PendingIntent each time. This does not happen. The parts of the Intent that are used for matching are the same one defined by the Intent#filterEquals(Intent).. If you use two Intent objects that are equivalent as per Intent#filterEqulas(Intent), then you 
will get the same PendingIntent for both of them. 

If you only need one PendingIntent active at a time for any of the Intents you will use, then you can alternatively use the flags FLAG_CANCEL_CURRENT or FLAG_UPDATE_CURRENT to either cancel or modify whatever current PendingINtent is associated with the Intent you are supplying. 

Also note that flags like FLAG_ONE_SHOT or FLAG_IMMUTABLE describe the PendingInent instnace and thus, are used to identify it. Any calls to retrieve or modify a PendingIntent created with theses flags will also require these flaggs to be supplied in conjunction with others. To retrieve an existing PendingInent created with FLAG_ONE_SHOT,  both FLAG_ONE_SHOT and FLAT_NO_CREATE need to be supplied. 

If you need multiple distinct PendingIntent objects active at the same time(such as to use as two notifications that are both shown at the same time), then you will need to ensure there is something that is different about them to associate them with different PendingInetnets. This may be any of the Intent attributes considered by Intent#filterEquals(Intent), or different request code integers supplied to getActivity(Context, int, Intent, int), getActivites(Context, int, INtent[], int), getBroadcast(Context, int, Intent, int), or getService(Context, int, Intent, int). 

## Security
By giving a PendingIntent to another application, you are granting it the right to perform the operation you have specified as if the other application was yourself(with the same permissions and identity). As such, you should be careful about how you build the PendingIntent: almost alwasy, for example, the base Intent you supply should have the component name explicityly set to one of your own components, to ensure it is ultimately sent there and nowhere else. 
