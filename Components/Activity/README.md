## Activity 
Activity is component that interacts with user providing a GUI. 


## Activity lifecyce
Is list of methods that the activity will run depending on where it is in the lifecycle. 
a description of each lifecycle method is included and when it runs.

Configuration changes
Configuration changes occur when the orientation of the device changes, a configuration chanmge result in reloading of the activity and a new layout depending on the orientation of the device. 

Process lifecycle
The Android system attemps to keep process around for as long possible, if memory needs to be freed up then process can be terminated. The four states of importance are listed here and will determine what process get terminated. 

Saving Persistent state
Saving persistent data will save the data as key value pair into a file so it can be loaded latter. 

Starting Activites and Getting Result
Use intents to start a new activity, if you want the new activity to return a result the current activity use use startActivityForResult(intent,int). 
