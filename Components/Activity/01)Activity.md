# Activity
An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(view). Activities can also be presented as floating windows, Muti-Window mode or embedded into other windows. There are two methods almost all subclasses of Activity will implmement. 

- OnCreate()
is where you initialize your activity. Call setContentView(int) with a layout resource defining your UI, and using findViewByid to retrieve the widgets in that UI that you need to interact with programmatically. 

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // the layout the activity will use
    setContetView(R.layout.activity_main)
    
}
```

- onPause()
is where you deal with the user pausing active interaction with the activity. Any changess made by the user should commited at this point. In this state the activity is still visible on screen. 
```
override fun onPause() {
    super.onPause()
}
```

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
