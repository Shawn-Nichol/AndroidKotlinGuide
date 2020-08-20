# Activity
An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(view). While activites are often preseneted to the user as full-screen windows, they can also be used in other ways: as floating windows, Muti-Window mode or embedded into other windows. There are two methods almost all subclasses of Activity will implmement. 

All activites should need to be declared in the Manifest file. 

- OnCreate()
is where you initialize your activity. Call setContentView(int) with a layout resource defining your UI, and using findViewByid to retrieve the widgets in that UI that you need to interact with programmatically. 

- onPause()
is where you deal with the user pausing active interaction with the activity. Any changess made by the user should commited at this point. In this state the activity is still visible on screen. 

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


## Configuration changes
If the configuration of the device changes then anything displaying a user interface will need to be update to match that configuration. Because Activity is the primary mechanism for interacting with the user, it includes special support for handling configuration changes.

Unless you specify otherwise a configuration change will cause your current activity to be destroyed, going through the normal activity lifecycle process of onPause(), onStop(), and onDestroy() as appropriate. If the activity had been in the foreground or visible to the user, once onDestroy() is called in that instance then a new instnace of the activity will be created, with whatever savedInstanceState the previous instnace had generated from onSaveInstanceState(bundle). 

This is done becuase any application resource, including layout files, can change based on any configuration value. Thus the only safe way to handle a configuration change is to re-retrieve all resouces, including layouts drawables, and strings. Becuase activites must already know how to save their state and re-create themeselves from that stae, this is a convenient way to have an activity restart itself with a new configuration. 

In some special cases, you may want to bypass restarting of your activity based on one or more types of configuration changes. This is done with the android:configChanges attribute in the manifest. For anytypes of configuration changes you say that you handle there, you will receive a call to your current activity's method instead of being restarted. If a configuration change involves any that you do not handle, however the activity will still be restarted and onConfigurationChange will not be called. 

## Starting Activites and Gettign Results
The startActivity(Intent) is used to start a new activity, which will be placed at the top of the activity stack. It takes a single argument, an Intent which describes the activity to be executed. 

Sometimes you want to get a result back from an activity when it ends. For example, you may start an activity that lets the user pick a person in a list of contacts: when it ends, it retuns the person that was selected. Todo this, you call the startActivityForResult(intent, int) version with a second integer parameter identifying the call. The result will come back through your onActivityResult(int, int, Intent) method

When an activity exits, it can call setResult(int) to return data back to its parent. It must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, it can optionally return back an intent containing any additional data it wants. All of this information appears back on the parent's Activity.onActivityResult(), along with the integer identifier it orignailly suplied. 

If a result fails for any reason the parent activity will receive a result with the code RESULT_CANCELED

## Saving Persistent state
There are generally two kinds of persistent state that an activity will deal with: shared document-like data and internal state such as user preferences. 

For content provider data, we suggest that activites use an "edit in place"  user model. That is, any edits a user makes are effectively made without requring an additional confirmation step. Supporting this model is generally a simple matter of following two rules. 
- When creating a new document, the backing database entry or file for it is created immediately. For example, if the user chooses to write a new email, a new entry for that email is created as soon as they start entering data, so that if they go to any other activity after that point this email will now appear in the list of drafts. 
- When an activity's onPause() is called, it should commit to the backing content provider or file any changes the user has made. This ensures that those changes will be seen by any other activity that is about to run. you will probably want to commit your data even more aggressively at key times during your activit's lifecycle: for example before starting a new activity, before finishing you own activity, when the use rswitches between input fields

This model is designed to prevent data loss when a user is navigating between activities, and allows the system to safely kill an activity at any time after it has been stopped. Note this implies that the user pressing back from your activity does not mean "cancel" it means to leave the activity with its current contents saved away. Canceling edits in an activity must be provied through some other mechanism, such as an explicit revert or undo option. 

See the content package for more information about content providers. These are a key aspect of how different activites invoke and propgagate data between themselves. 

The activity class also provides an API for managing internal persistent state associated with an activity. This can be used for example, to remember the user's preferred inital display in a calendar or the user's default home page in a web browser. 

The activity class also provides an API for managing internal persistent state associated with an activity. This can be used, for example to remember the user's preferred initial display in a calendar or the user's default home page in a web browser. 

Activity persistent state is manged with the method getPreferences(int), allowing you to retrieve and modify a set of name/value pairs associated with the activity. To use preferences that are shared across multiple application components, you can use the underlying Context#getSharedPreferences to retieve a preferences object stored under a specific name

## Process lifecycle
 The andoird system attempts to keep an application process around for as long as possible but eventually will need to remove old processes when memory runs low. As described in Activity lifecycle, the decision about which process to remove is tied to the state of the user's interaction. In general, there are four states a process can be in based on the activites running in it, listed here in order or impmortance. The system will kill less important processes before it resorts to killing more important processes.
 
 1) The foreground activity (activity at the top of the screen) is considered the most important. Its process will only be killed as a last resort, if it uses more memory than is available on the device. Generally at this point the device has reached a memory paging state, so this is required in order to keep the user interface responsive. 
 
 2) A visble activity(an activity that is visble to the user but no in the foreground, such as one sitting behind a foreground dialog or next to other activites in multi-window mode) is considered extremely important and will not be killed unless that is required to keep the foreground activity running.
 
 3) A background activity (an activity that is not visible to the user and has been stopped) is no longer critical, so the sytem may safely kill its process to reclaim memory for other foreground or visible processes. If a process is killed, when the user navigates back to the activity its onCreate(bundle) method will be called with the savedIstance State it had previously supplied in onSaveInstanceState(Bundle) so that it can restart itself in the same state as the user last left it. 
 
 4) An empty process is one hosting no activites or other application components(such as service or BroadcastReceiver classes). These are killled very quickly by the system as memory becomes low. For this reason, any background operation you do outside of an activity must be executed in the context of an activity BroadcastReceiver or Service to ensure that the system knows it needs to keep your process around. 
 
Sometimes an activity may need to do a long-running operation that exists independently of the activity lifecycle itself. An exmaple may be a camera application that allows you to upload a picture to a website. The upload may take a long time, and the appication should allow the user to leave the application while it is executing. To accomplish this, your Activity should start a service in which the upload takes place. this allows the system to properly priortize your process for the duration of the upload, independent of whether the original activity is paused, stopped or finsished. 

