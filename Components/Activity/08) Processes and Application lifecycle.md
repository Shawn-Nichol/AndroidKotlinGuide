# Processes an Application lifecycle
In most cases, every android application runs in its own linux process. HTis process is created for application when some of its code needs to be run and will remain running until it is  no longer needed and the system needs to reclaim its memory for use by other applications. 

A fundemental feature  of android is that an applictioan process's lifetiem is not driectly controlled by the application itself. Instead, it is determined by the system througha combination of the parts of the application that the system knows are running, how improtant these things are to the user, an dh ow much overall memory is aviable in the system. 

It is important to understand thathow different applicationcomponets impact the lifetime of the application's process. Not using these componets correctly can result in the system killing the application's process while it is doing important work. 

A common problem is a BroadcastReceiver that starts a thread when it receives an Intent in its BroadcaastReceiver.onRecieve() and returns from the function. Once it returns, the system considres the BroadcastREceiver to be no longer active and its hosting processess no longer needed. The system may kill the process at an any time to reclaim memory. The solution to this problem is to schedule a JobSErvice fromthe BroadcastReceiver, so the system knows that there is still active work being done in the process. 
