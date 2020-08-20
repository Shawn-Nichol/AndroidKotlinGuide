## Activity Lifecycle
Activites in the system are managed as activity stacks. When a new activity is started, it is usually placed on the top of the current stack and becomes the running activity the prevoius activity remains below it in the stack, and will not come to the foreground again until the new activity exits. There can be one or multiple activity stacks visible on screen. 

An activity has essentially four states. 
- Active: If an activity is in the foreground of the screen. This is usually the activity that the user is currently interacting with. 
- Visible: If an activity has lost focus but is still presneted to the user, it is visible. It is possible if a new non-full-sized or tranparent activity has focus on top of your activity, another activity has higher position in multi-window mode, or the activit itself is not focusable in current windowing mode. Such activity is completely alive. 
- Hidden or stopped: If an activity is completely obscured by another activity, It still retains all state and member information, however, it is no longer visible to the user. 
- The system can drop the activity from memory by either asking it to finish, or simply killing its process, making it destroyed. When it is displayed again to the user, it must be completely restarted and restored to its previous state. 

There are three key loops you may be interested in monitoring within your activity

- The entire lifetime of an activity happens between the first call to onCreate through to a single final call to onDestroy(). An activity will do all setup of global states in onCreate(), and release all remaining resources in onDestroy(). For example if it has a thread running in the background to download data from the network, it may create that thread in onCreate and then stop the thread in onDestroy()

- The visible lifetime of an activity happens between a call to onStart() until a corresponding call to onStop(). During this time the user can see the activity on-screen, though it may not be in the foreground and interacting with the user. Between these two methods you can maintain resources that are needed to show the activity to the user. For example, you can register a a BroadcastReceiver in onStart() to monitor for changes that impact your UI, and unregister it in onStop() when the user no longer sees what you are displaying . The onStart() and onStop() methods can be called multiple times, as the activity becomes visible and hidden to the user. 

- The foreground lifetime of an activity happens between a call to onResume() until a corresponding call to onPause(). During this time the activity  is invisible, active and interacting with the user. An activity can frequently go between the resumed and pasue states for example when the device goes to sleep, when an activity result is delivered, when a new intent is delivered so the code in these methods should be failry lightweight. 

The entire lifecylce of an activity is defined by the following Activity methods. All of these are hooks that you can override to do appropriate work when the activity changes state. All activites will implement onCreate()  to do their intial setup; many will also implement onpause() to commit changes to data and prepare to pause interacting with the user, and onStop() to handle no longer being visible on screen. You should alwasys call up to your superclass when implementing these methods. 

```
public class MyActivity extends ApplicationContext {

  protected onCreate(Bundle savedInstanceState)
    called when the activiy is first created. This is where you should do all of your normal static set up: create views, bind data to lists etc. This method also provides you
    with a bundle containing the activity's previously frozen state, if there was one. 
    Followed by onStart()

  protected void onRestart()
    Called after your activity has been stopped, prior to it being started again.
    Followed by onStart()

  protected void on start()
    Called when the activity is becomming visible to the user. 
    Followed by on Resume(), if the activity comes to the foreground, or onStop() if it becomes hidden. 
    
  protected void onResume()
    called when the activity will start interacting with the user. At this point your activity is at the top of its activity stack, with user input going to it. 
    Followed by onPause()
  
  protected void onPause()
    Called when the activity loses foreground state, is no longer focusable or before transition to stopped/ hidden or destroyed state. The activity is still visble to user,
    so it's recommened to keep it visually active and continue updating the UI. Implementations of this method must be very quick becuase the next activity will not be resumed
    until this method retuns. 
    Followed by either onResume() if the activity returns back to the front, or onStop() if it becomes invisible to the user. 
  
  protected void onStop()
    Called when the activity is no longer visible to the user. This may happen either becuase aa new activity is being started on top, an existing one is being brought in front
    of this one, or this one is being destroyed. This is typically used to stop animations and refreshing the UI.
    Followed by either onRestart() or onDestroy
    
  protected void onDestroy()
    The final call you receive before your activity is destroyed. This can happen either beauxe the activity is finsishing(someone called Activity#finsih on it), or because the
    system is temporarily destroying this instance of the activity to save space. You can distingush between these two scenarious with Activay#isFinishing. 
}
```

Write persistant data in onPause() as the later methods me not run. 
