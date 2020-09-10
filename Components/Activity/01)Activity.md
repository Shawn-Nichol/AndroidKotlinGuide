# what is an Activity
An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(view). Activities can also be presented as floating windows, Muti-Window mode or embedded into other windows. There are two methods almost all subclasses of Activity will implmement. 

## OnCreate()
is where you initialize your activity. Call setContentView(int) with a layout resource defining your UI, and using findViewByid to retrieve the widgets in that UI that you need to interact with programmatically. 

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // the layout the activity will use
    setContetView(R.layout.activity_main)
    
}
```

## onPause()
is where you deal with the user pausing active interaction with the activity. Any changess made by the user should commited at this point. In this state the activity is still visible on screen. 
```
override fun onPause() {
    super.onPause()
}
```


## Manifest
All activites need to be declared in the Manifest file. 

AndroidManifest.xml
```
<application
    ...
    <activity android:name=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN"/>
            <category android:name="android.intent.category.LAUNCHER"/>
            </intent/fillter>
    </activity>
    
    
    <activity
        android:name="ActivityTwo"
        android:label="ActivityTwo"
        android:theme="@style/appTheme.NoActionBar">
    </activity>
</application
```

## Four States
- Active: If an activity is in the foreground of the screen. This is usually the activity that the user is currently interacting with. 
- Visible: If an activity has lost focus but is still presneted to the user, it is visible. It is possible if a new non-full-sized or tranparent activity has focus on top of your activity, another activity has higher position in multi-window mode, or the activity itself is not focusable in current windowing mode. Such activity is completely alive. 
- Hidden or stopped: If an activity is completely obscured by another activity, It still retains all state and member information, however, it is no longer visible to the user. 
- The system can drop the activity from memory by either asking it to finish, or simply killing the process, making it destroyed. When it is displayed again to the user, it must be completely restarted and restored to its previous state. 
