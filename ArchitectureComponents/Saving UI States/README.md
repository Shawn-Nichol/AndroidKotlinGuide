## What is Saving UI States
Preserving and restoring an activity's UI states in a timely fashinon across sytem-initiated activity or application destruction is a crucial part of the user experince. In these cases the users expects the UI state to remain the same, but the system destorys the activty and any state stored in it. 

To brdige the gap between user expectation and system behavior use a combination of ViewModel objects, the onSaveInstanceSTate() methods and or local storage top ersist the UI state across such application and activity instance transitions deciding how to combine these options depends on the complexity of your UI data, use cases for ayour app, and consideration of speed of retrieval versus memory usage. 

Regardless of which approach you take, you should ensure you app meets user expectations with respect to thier UI state, and provides a smooth snappy UI (avoid lag time in loading data into the UI, sespecially after frequently occuring configuration changes, like rotation).  In most cases you should use both ViewModel and onSavedInstanceState()

## User expectations and dsystem behavior
Depending upon the action a user takes they either expect that activit  state to be cleared or the state to be preserved. In some cases the sytem automtaclly does what is expected by the user. In other cases the system does the opposite of what the user expects. 

User-inititated UI state dismissal
The user expects that when they start an activity, the transient UI state of that activity will remain the same until the user completely dismisses the activity. The user can completely dismiss an activty by. 


- pressing the backbutton
- swiping the activity off of the overview (recent) screen
-  navigating up from the activity. 
- killing the app from setting screen.
- COmpleting some sort of finishing activity (which is backed by activity.finish().

The user's assumption in thses complete dismissal cases is that they have permanently navigated away from the activity, and if they re-open the activity they expect the activity to start from a clean state. The underlying system behavior for these dismissal scenarios matches the user expectation the activity instance  will get destroyed and removed from meomry, along with any state stored in it and any saved instance state record assodicated with the activity. 

There are some exception to this rule about complete dismissal for example a user might expect to a browser to take them to the exact webpage they were looking at before they exited the browser using the back button.

## System-initiated UI state dismissal
A user expects an activity's UI statte to remain the same throut a configuration chagne, such as rotation or swithing into multip-window mode. However, by default the system destorys the activity when such a configuration change occurs, wiping away anay UI stae stored int eh activity instance. To learn morea bout device donfigurations, see the configuration reference page. Note, it is possible to override the default behavior for configuration changes

A user also expects your activity's UI state to remain the smae if they temprorarily switch to a different app and then come back to your app later. For example, the user performs a search in your search activity and then presses the home button or answers a phone call -when they return  to the serach activity they expect to find the searhc keyword and results still there, exactly as before. 

In this scenario, your app is place in the backgorund the system does its best to keep your app process in memory. However, the sytem does it sb est to keep your app processs in meomry. However, the system may destory the application process while the user is away interacting with other apps. In such a case, the activity instance is destoryed, along with any state store in it. When the user relaunches the app, the activity is unexpectedly in a clean state. Lo learn more about prodess death, see Process and Application LifeCycled. 

## Options for presvering UI state. 
When the user's expectations about UI state do not match default system behavior, you must save and restore the user's UI state to ensur ehtat the system-initiated destruction is transparent to the user. 


                            |        ViewModel          |     Saved instance state      | Persistent Storage. 
Storage location            | In Memory                 | serialized to disk            | on disk or network
survivies config chagnes    |  yes                      |       yes                     |       Yes
Process Death               |  no                       |       yes                     |       yes
Survives activity dismissal |  no                       |       no                      |       yes
Data Llmiitation            | complex object are find   | Only for primitive types      | Only limited by disk space
                            |  but space is limited by  | and simple, small objecs      | cost / time of retrieval from
                            |   available memory        | such as strings               | the network resource
                            |                           |                               | 
Read/write time             | Quick memory access       | Slow requires                 | slow (requires disk access or 
                            | only                      | serializatin/deserialization  | network transactions
                            |                           | and disk access





managing UI State: Divide and conquer. 

You can efficiently save and restore UI state by dividing the work among the various types of persistence mechansisms. In most cases, each of these mechansisms should store a different type of data used in The activity, based on the tradeoffs of data complexity, access speed, and lifetime. 

- Local persistence: Stores all data you don't want to lose if you openand close the activity. 
  - example a collection of song objects which could include audio files and metadata. 

- ViewModel: Stores memory all the data needed to display the associated UI controller. 
  - Example: The song objects of the most recent search and the most recent serach query. 

- onSaveInstanceState: Stores a small amount of data needed to easily reload activity state if the system stops and then recreates the UI controller. Instead of storing complex objects here, persist the complex objects in local storage and store a unique ID for these objects in onSaveInstanceState()
  - Example: Storing the most recent search query. 
  
As an example consider an acitivty that allows you to search through your library of songs. Here's how different events should be handled.

When the user adds a song, the ViewModel immediately delegates persisting this data locally. If this newly added song is something that should be shown in the UI, you should also update the data in the ViewModel object to reflect the addition of the song. Remember to do all database inserts off the main thread. 

When the users rearches for song whatever complex song data you load from the database for the UI controller should immediately stored in the ViewModel object. you should also save the search query itself in the ViewModel object. 

When the activty goes into the background, the system calls onSaveInstanceState(). You should save the search query in the onSaveInstanceState() bundle. This small amount of data is easy to save. It's also all the information you need to get the activty back into its current state. 

