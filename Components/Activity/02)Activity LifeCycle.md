# Activity Lifecycle
Activities in the system are managed as an activity stacks. When a new activity is started, it is usually placed on the top of the current stack and becomes the running activity the prevoius activity remains below it in the stack, and will not come to the foreground again until the new activity exits. There can be one or multiple activity stacks visible on screen. 

### onCreate 
This callback fires when the system first creates the activity, in onCreate you perfrom basic application startup logic that should happen only once for the entire life of the activity. This method receives the savedInstanceState, which is a bundle object containt the activity's previosly saved state. If the activity has never existed before, the value of Bundle object is null. 

```
override fun onCreate(savedInstanceState: Bundle?) {
  // Call the super class onCreate to complete the creation of activity like view hierachy
  super.onCreate(savedInstanceState)
  
  // Set the layout file
  setContentView(R.layout.main_activity)
  
 
}
```
After onCreate finishes the activity enters onStart() and onResume() in quick succession. 

### onStart
OnStart makes the activity visible to the user, as the app prepares for the activity to enter the foreground and become interactive. Thisapp initializes the codde that maintans the UI.

```
override fun onStart() {

}
```
next the activity will enter the onResume()

### onResume
The app comes to the foreground, and then the system invokes onResume(). The state in which the app interacts with the user. The app stays in this state until something happens to take focus away from the app. Such as event might be fore instance, receiving a phone call, the uer's navigationg to another activity or the device screen's runging off. 

When an intteruptive even occurs the activity enters the paused state, and the system invokes the onPause() callback. If the system returns to the onResumed state fomr the puased state, the system once again calls onResume() method. For this reason , you should implement onResume() to initialize componets that you release during onPause(), and perfrom any other initializations that must occur each time the activity enter the resume state. 
```
override fun onResume() {

}
```

### onPause
When the activity leaves the foreground it will enter the onPause() state. Use onPause() to adjust operations that hsould not continue (or should continue in moderation) while the Activity is in the Paused state, and that you expect to resume shortly. 

onPause can also be used to realease system resources, handles to sensors or any resource that may affect battery life while your activity is paused and the user does not need them. 

```
override fun onPause()
```

onPause execution is very brief, and does not provider enougth time to perfrom save operartions. You should use onStop(). 

Completion of the onPause() method does not mean that the activity leaves the pause state. Rather, the activity remains in this state untileither the activity resumes or beomces completely invisble to the user. If the activity resumes, the system once agian invokes the onResume() callback. 

### onStop
When your activity is no longer visible to the user, it has entered the stopped state, and thes system invokes the onstop() callback. The app should release or adjust resources that are not needed while the app is not visible to the user. FOr example, your app might pause animations or switch from fine-grained to coarse-grained location updates. Using onStop() instead of onPuase() ensures that UI--related work continues, 

```
override onStop() {
  // Call the superclass method first
  super.onStop()
}
```
When the activity enters the stopped statet, the activity object maintains all state and member information, but is not attached to the window manager. When the activity resumes, the activity recalls this information. You don't need to re-intialize componets that were created during any of the callback methods leading up to the Resumed state. The system also keeps track of the current state of each View object in the layout, so if the user entered text into an EditText widget, that content is  retained so youdon't need to save and restore it. 


### onDestory()
onDestroy is called before the activity is destoryed. Thes sytem invokes this callbeack either becauese
1) the activity is finsihing(due to the user completely dimissing the activity or due to finish() being called onthe activity

2) the sytem is temporarilly destorying the activit due to a configuration change.
```
override fun onDestory() {

}
```

## Saving and restoring
A user expects an activty's UI state to reamin the same throughout configuration changes. 

### Instance state
IF your activity is destoryed by a backbutton or finish() method theyn you don't need to save information. If the activity is destoyred due to system constaints(configuration changed, low memory), then the system recreates the activity with saved datat that describes the state of the activity when it was destoryed. 

The saved data the system uses to restore is called the instance state and is a collection of key-value pairs stored in a bundle object. Note a bunlde object isn't appropritate for preseving  more than a trival amount of data beccuase it requires serilaization on the main thread and consumes system-rocess memory. To presever more than a very small amount of dataa, you should take a combined approach to preserving data, using persisten local storage, the onSaveInstanceState() and ViewModel.


### onSavedInstanceState()
onSavedInstanceState is called as the activity begins to stop. Infomration is saved to a Bundle object using a key-value pair. 
```
override fun onSaveInstnaceState(outState: Bundle?) {
  outState?.run {
    putString(FIRST_KEY, myVaraible)
    putString(SECOND_KEY, textView.text.toString())
    
    // allways call superclass so it can save the view hierarchy state
    super.onSaveInstanceState(outState)
  }
}
```
onSavedInstanceState isn't called when the user explicitly closes the acctivit or in other when finish() is called. 

### restore activty UI stae
When an activity is recreated after being destroyed, you can recover the saved data instance state from the ubnlde that the system passes to your activity. Both in onCreate and onReseotreInstanceState() callbacks. Because onCreate method is calle dwhether the system is creating a new instance of your activity or recreating a previous one, you must check whether the state bundle is null before you atttempt to read it. If it is null, then the system is creatinga new instance of the activity, instead of restoring a previous one that was destoryed. 
```
override fun onCreate(savedInstanceState: Bundle?) {
  ...
  // Check if theres a savedInstanceState, if so restore the values. 
  if(savedInstanceState != null) {
    with(savedInstanceState) {
      // Restores values of members from saved state.
      currentScore = getInt(STATE_SCORE)
    }
  } else {
    // Initialize members with default values. 
  }
}
```

You can also restore the state during onrestoreInstanceState, the system will call after onStart() but only if there is a saved state to restore, so you don't need to check whether the bundle is null

```
override fun onRestoreInstanceState(savedInstanceState: Bundle?) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState)

    // Restore state members from saved instance
    savedInstanceState?.run {
        currentScore = getInt(STATE_SCORE)
        currentLevel = getInt(STATE_LEVEL)
    }
}
```

## Activity Loops
There are three key loops you may be interested in monitoring within your activity.

- The entire lifetime of an activity happens between the first call to onCreate() through to a single final call to onDestroy(). An activity will do all setup of global states in onCreate(), and release all remaining resources in onDestroy(). For example if it has a thread running in the background to download data from the network, it may create that thread in onCreate and then stop the thread in onDestroy()

- The visible lifetime of an activity happens between a call to onStart() until a corresponding call to onStop(). During this time the user can see the activity on-screen, though it may not be in the foreground and interacting with the user. Between these two methods you can maintain resources that are needed to show the activity to the user. For example, you can register a a BroadcastReceiver in onStart() to monitor for changes that impact your UI, and unregister it in onStop() when the user no longer sees what you are displaying . The onStart() and onStop() methods can be called multiple times, as the activity becomes visible and hidden to the user. 

- The foreground lifetime of an activity happens between a call to onResume() until a corresponding call to onPause(). During this time the activity  is invisible, active and interacting with the user. An activity can frequently go between the resumed and pasue states for example when the device goes to sleep, when an activity result is delivered, when a new intent is delivered so the code in these methods should be failry lightweight. 

The entire lifecylce of an activity is defined by the following Activity methods. All of these are hooks that you can override to do appropriate work when the activity changes state. All activites will implement onCreate()  to do their intial setup; many will also implement onpause() to commit changes to data and prepare to pause interacting with the user, and onStop() to handle no longer being visible on screen. You should alwasys call up to your superclass when implementing these methods. 



Write persistant data in onPause() as the later methods me not run. 
