# Process lifecycle
In most cases, every android application runs its own Linux process. THis process is created for the application when some of its code needs to be run, and will remain running until it is no longer needed and the systme needs to reclaim its memory for use by other applications. 

An unusall and fundamental feature of android is that an application process's lifetime is not directly controlled by the application itself. Instead, it iis determined by the system through a combination of the parts of the application that the system knows are running, how important these things are to the user, and how much overall memory is available in the system. 

A common example of a process life-cycle bug is a BroadastReciever that starts a thread when it receives an iNtnet in its BroadcastReceiver.onReceive() method, and then returns tfrom the function. Once it returns, the system considers the BroadcastReceiver to be no longer active, and thus, its hosting process no longer needed (nuless other application components are active in t it)> So the system may kill the process at any time to reclaim memory, and in doing so,  it terminates the spawned thread running in the process. The solution to this problem is typically to schedule a JobService from the BroadcastReceiver, so the sytme knows that there is still active work being doen in the process. 

To determine which processs should be killed when low on memory, Android places each process into a importance hierarchy base on the ocmponents running and the state of those omponents. These process types are listed in order of imporance.

1) A foreground proces si sone that is required for what the user is currently doing. Various application components can cause its containing process to be considered foreground in different ways. A process is considered to be in the foreground if any of the following conditions hold

- It is running an Activity at the top of the screen that the user is interacting with.
- It has a BroadcastReciever that is currently running 
- It has a Service that is currently executing code in one of its callbacks

There will only ever be a few such process in the system, and these will only be killed as a loast resort if memory is so low that not even these process can continue to run. Generally, at this point the device has reached memory pagingin state, so this action is required in order to keep the user interface responsive. 

2. A visible process is doing work that the user is currently aware of, so killing it would have a noticeable negative impact on the user experince. A process is considered visible in the following conditions:

- It is runing an Activity that is visible ot the user on-screen but no tin the foregorund. This may occur for example, if the foreground Activity is displayed as a dialog that allows the previos Activity to be seen behind it. 
- It has a Service that is running as a foreground service,through SErvice.startForeground()
- It is hosting a service that the system is using for a particular feature that the user is aware, such as a live wallpaper, input method service 

The number of these proceses running in the systme is less bounded than foregournd processes, but still relatively controlled. These processes running in the system is less bounded than foregournd processes, but stilll relatively controlled. These processes are considered extremely important and will not be killed unless doing so is required to keep all foreground process running. 

3. A servide process is one hold a SErvice that has been started with the startService() method. Through these processes are not directly visible to the user, they are generally doing things that the user cares aboutn(such as background network data upload or download), so the system will always keep such processes running unless there is not enough memory to retain all foreground and visible processes. 

Services that have been running for a long time may be dmeoted in importance to allow stheir process to drop to the cached list. This helps avoid situations wher long running services that use excessive resources prevent the ssytem from delivering a good user experince. 

4.  A cache process is one that is not currently needed, so the system is free to kill it as desired when resources like memory are needed elsewhere, iIn a normally behaving system, these are the only porocesses involved in rsource maagement: a well running system willl have multiple cached processees always available and regularly kill the oldest ones as needed. Only in very critical situations will the system get to a point where all chaced processes are killed and it must start killing service processes. 

These processes often hold one or more Activity instances that are not currently visible ot the user (the onStop() method has been called and returned. Provided they mimpleement their Activity life-cycle correctly. Acitvity for more details, when the system kills such processes it willnot impact the user's experience when returning to that app: it can restore the previosly saved state when teh associated activity is recreated in a new process.

These processes are kept in a list. The exact policy of ordering on this list is animplementation detail of the platform, but generally it will try to keep more useful processes before other types of processes. Other policies for kiliing processes may also be applied hard limits on the hnumber of processes allowed, limits on the amount of time a process can stay continually cached. 


When decing how to calssify a process, the system will base its decisionon the most important level found among all the components currently active in the process. A process's priority may also be increased based on other dependencies a process has to it. For example, if process A is bound to a Service with the Context.BIND_AUTO_CREATE flag or is using a ContentProvider in process B, then process B's classification will always be at least as important as process A's. 
