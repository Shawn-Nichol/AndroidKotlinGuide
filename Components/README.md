# App components

App componets are the essential bulding blocks of an Android app. Each component is an entry point through which the system or a user can enter your app. Some conponets dpened on others. There are four different types of app componets Activies, Services, Broadcast recievers, Content providers. Each type serves a distnct purpose and has a distinct lifecycle that defines how the conponet is created and destroyed. 

## Activies
An Acitvity is the entry point for interacting with the user. It represents a single screen with a new user interface for example an email app might have one activit that shows a list of new emails another acitvity to compose an email, and another acitvity fore reading emails. Although the activites work together to form a cohesive user experience, each one is indepenedent of the others. As such a different app can start any one of these activites if allowed. An activity facilitates tteh following key interactions between system and app:

- Keeping track of what the user currently cares about(whats on the screen) to ensure that the sytem keeps running the process that is hosting the activity. 
- Knowning that previously used proceseses contain things the user may return to (stopped activites), and thus more highly prioritze keeping those processes around. 
- Helping the app handle having its process killed so the user can return to activites with their previous state restored. 
- Providing a way for apps to implment user flows between each other, and for th system to coordinate these flows. 

You implment an acitivty as a subclass of Activity class.


## Services

A service is a general-purpose entry point for keeping an app running in the background for all kinds of reasons. It is a conponent that runs in the background to perform long-running operations or to perform work for remote processes. A service does not provide a user interface. For example, a service might play music in the background while the user is in a different app, or it might fetch data over the network withougt blocking user interaction with an activity.. Another conponent, such as an activity, can start the service an let it run or bind to it in order to interact with it. There are actually two very sdistinct semantics services tell the system about how to manage an app: Starte services tell the system to keep them running until their work is conpleted. This could be to sync some data in the background or play music even after th euser leaves the app. Syncing data in teh background or playing music also represent two different types of statred services that modify how the system handles them:

- Music playback is something the user is directly aware of, so the ap tells the system this by syaing it wants to be foregroudn with a notification tell teh user about it; in this case the system knows that it should try really hard to keep that service's process running, becuase the user will be unhappy if it goes away. 
- A regular backgroun service is not something the user is directly aware as running, so the systme has more freeedom in managing its process. It may allow it to be killed (and then restarting the service sometime later) if it needs RAM for things that are of more immediate concern to the user. 

Bound services run becuase some other app has said that it wants to make use of the service. This is basically the service profviding an API to another process. The system thus knows there is a dependency between these processes, so if process A is bound to a service in process B, it knows that it needs to keep porcess B running for A. Further, if process A is somethign the user cares about, then it also knows to treat process B as somethign the user also cares aobut. Becuase of their flexibility, services have turned out to be a really useful buliing block for all kinds of higher-level system concepts. Live wallpapers, notification listeners, screen savers, input methods, acccessibility implement and the sytem binds to when they should be running. 

A service is implemented as a subclass of Service 

## Broadcaste receivers 

 A broadcast receiver is component that enables the system to deliver events to the app outside of a regular user flow, allowing the app to respond tgo system-wide broadcast announcemnts. Because broadcast receivers are another will-defined entry into the app, the system can deliver broadcast even to apps that aren't currently running. so for example, an app can schedule an alarm to post a notification to tell the user about an upcoming event... and by delivering that alarm to a BroadcastReciever of the app, there is no need for the app to remain running until the alarm goes off. Many broadcast origniate from the system- for example, a brodcast announcing that th3e screen has turned off, the battery is low or a picture was captured. Apps can also initiiate broadcasts- for example to let other apps know that some data has been downloaded to the device and is available for them to use. Although broadcast receivers don't display a user interface, they may create a status bar notification to alert the user when a broadcast event occurs. More connonly, htough, a broadcast receiver is just a gateway to other conponents and is intended to do a very minimal amount of work. For instance, it might schedule a JobService to perform some work based on the event with JobScheduler. 
 
 A broadcast receiver is implemented as a subclass of broadcast reciever and each broadcast is delivered as an intent object.
 
 ## Content providers
 
 A content provider managse a shared set of app data that you can store inthe file system, in a SQLite database, on the web or on  other persistent storage location that your app can access. Through the content provider, other apps can query or modify the data if the content provider allows it. For example, the Android system provides a content provider that manages the user's contact information. As such, any app withthe proper permissions can query the content provider, such as ContactContract.Data, to read and write information about a particular person. It is tempting to thingk of a content provider as an abstraction on a database, becuase there is a lot of API and support built in to them for that common case. However, they have a differenet core purpose from a system-design perspective. To the sytem, a content provider is an entry point into an app for publishing named data items., identifiied by a URI scheme. Thus an app can decide how it wants to map the data it contains to a URI namespace, handing out those URIs toother entities which can in turn use them to access the data. THere are a few particular things this allows the system to do in managing an app
 
 - Assigning a URI doesn't require that the app remain running, so URIs can persist after their owning apps have exited. The systme only needs to make sure that an owning app is still running when it has the retrieve the app's data fromt he corresponding URI. 
 - These URIs also provide an important fine-grained security model. For example, an app can place the URI for an image it has on the clipboard, but leave its content provider locked up so that other apps cannot freely access it. When a secocnd app attempts to access that URI on the clipboard, the systme can allow that app to access the data via a temporary URI permission grant so that it is allowed to access the data only behind that URI, but nothing else in the second app. 
 
 Content providers are also  useful for reading and writing data that is private to your app and not shared. 
 
  A Content provider is implemented as a sbuclass of ContentProvider and must implmenet a standard set of APIs that enable other apps to perform transactions. 
  
  A Unique aspect of the Android  system design is that any app can start another app's component. For example if you want the user to capture a photo withthe device camers, ther's probalbly another app that does that and your app can use it isntead of developing an activity  to capture a photo yourself. You don't need to incorporate or even link to the code fromt he camera app. Instead, you can simply start the activity inthe camera app that captures a photo. When complete, the photo is even returned to your app so you can use it. To the user, it seems as if the camera is actually a part of your app. 
  
  When the systme starts a conponent, it starts the process for that app if it's not already running and instantiates the classes needed for the conponent. For example, if your app starts the activity in the camera app that captures a photo, that activity runs in the process that belongs to the camera app, not in your app's process. Therefore, unlike apps on most other systems, Android apps don't have a single entry point
  
  Becuase the systme runs each app in a separate process with file permissions that restrict acces to other apps, you app cannot directly activiate a conponent in another app, deliver a message to the system that specifies your intent to start a particuolar component. The systme then activates the conponent for you. 
  
## Activating Component

Three of the four component types activites services and broadcast receivers are activated by an asynbchronous message called an intent. Intents bind individual components to each other at runtime. You can think of them as the messageengers that request an action fromother conponetns, whether the conponent belongs to your app or another. 

An intent is created with an Intent object which defines a message to activate either a specific conponent or specific type of conponent(impliicit intent)

For activites and services, an intent defines the action to perform and may specify the URI of the data to act on, among other things that the conponent being started might need to know. For example an intent might convey a request for an activity to show an image or to open a web page. In some cases you can start an activity to receive a result, in which case the activity also returns the results in an Intent. For example you can issue an intent to let the user pick a personal contact and have it returned to you . THe return intent includes a URI pointing to the chosen contact. 

For broadcast receivers, the intent simply defines the announcement being broadcast. For example a broadcast io indicate the device battery is low includes only a known action string that indicates batter is low. 

Unlike activites, services and broadcast receivers, ontent providerrs are not activated by intents. Rather, they are activated when targeted by a request from a ContentResolver. The content resolver handles all direct transactions with thte content provider so that the conponent that's performing transactions with the provider doesn't need to and instead calls methods on teh ContentResolver object. This leaves a layer of abstraction between the content provider and the onponent requesting information.

There are separate methods for activating each type of conponent: 
- You can start an activity or give it something new to do by passing an Intent to startActivit() or startActivityForResult()
- You can use the JobSceduler class to schedule actions. 
- You can initiate a broadcast by passing an Intent to methods such as sendBroadcast() sendOrderBroadcast(), or sendStickyBroadcast().
- You can perform a query to a content provider by calling query() on a ContentResolver. 
