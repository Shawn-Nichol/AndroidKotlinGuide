# Intent 
An Intent is a messaging object you can use to requrest an action from another app component. These are the three fundamental forms of communication

- Starting an Activity, you can start a new instance of an activity by passing an Intnet to startActivity(). The intnet descibesthe activity to start and carries any necessary data. 

- Starting a service you can start a a service to perform a one-time operation by passing an intent to startService(). The intent describes the service to start and carries any necessary data. 

- Delevering a broadcast, the systme delivers varous broadcasts for system events, such as when the system boots up or the device starts charging, you can deliver a broadcat to other apps by passing an Intent to sendBroadcast() or sendOrderedBroadcast()

An Intent provides a facility for performing late runtime binding between the codde in different application. Its most significant use is in the launching of activites, where it can be thought of as the glue betweeen activites. It is basically a passive data structure holding an aabastracct description of an action to be performed. 

## Inttent Structure
The primary pieces of information in an intent are
- Action: the general action to be performed, such as ACTION_VIEW, ACTION_EDIT, ACTION_MAIN
- Data: the data to operate on such as a person record in the contact database. 

## Types of Intent
- Explicit intents specify which application will satisfy the intent, by supplying either the target app's package name or a fully-qualified component class name. You'll typically use an explicit intent to starta component in your own app, becuase you know the class name of the activity or service you want to start. 

- Implicit intent: Does not name a specific component, but instead declares a general action to perform, which allows a component form another app to handle it.


When you use an implicit intnet, the Android system finds the appropriate component to start by comparing the contents of the intent to the intent filters declared in the mainfest file or other apps on the device. If the intent matches an intent fileter, the system starts that component and delivers it the intent object. If multiple intent filters are compaatible the systme displays a dialog  so the user can pick which app to use. 

An intent filter is an expression in app's manifest file that specifies the type that component would llike to reeive. For instance by declaring an intent filter for an activity, you make it possible for other apss to directly start your activity with a certain kind of intent. Likewise if you so not declare any intent filters for an acivity then it can be started only with an explicit itnnet. 

## Building an Intent
