# Activity
An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(view). While activites are often preseneted to the user as full-screen windows, they can also be used in other ways: as floating windows, Muti-Window mode or embedded into other windows. There are two methods almost all subclasses of Activity will implmement. 

All activites should need to be declared in the Manifest file. 

- OnCreate()
is where you initialize your activity. Call setContentView(int) with a layout resource defining your UI, and using findViewByid to retrieve the widgets in that UI that you need to interact with programmatically. 

- onPause()
is where you deal with the user pausing active interaction with the activity. Any changess made by the user should commited at this point. In this state the activity is still visible on screen. 
