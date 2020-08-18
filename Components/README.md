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
