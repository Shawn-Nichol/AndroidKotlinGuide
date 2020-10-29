# Process lifecycle
The Android system will attempt to keep the process hosting a service around as long as the service has been started or has clients bound to it. When running low on memory and needing to kill existing processes, the priority of a process hosting the service will be the higher of the following possibilities

- If the service is currently executing code in its onCreat(), onStartCommand() or onDesetory methods, then the hosting process will be a foreground process to ensure htis code can execute without being killed. 

- If the serivce has been started, then its hosting process is considered to be less important than any processes that are currently visible to the user on-screen, but more important than any process not visible. Becuase only a few processes are generally visible to the user, this means that the service should not be killed except in low memory conditions. However, since the user is not directly aware of a background service, in that state it is considered a vlid candidate to kill and you should be prepared fo this to happend in particular ong-running services will be increasingly likley to kill and are guaranteed to be killed if they remain started long enough. 

- If there are clients bound to the service, then the service's hostin gprocess is never less important than the most important client. That is, if one of its clients is visible to the user, then the service  itself is considered to be visible. The way a client's importance impacts the service's importance can be adjusted. 

- A started service can use the startForegounr(int, notification) API to put the serive in a foregroun state, where the sytem considers it to be something the user is actively aware of and thus nota  candidate for killling when low on memory. (it is still theoretically possible for the service to be killed under extreme memory pressure fomr the current foregroudn application, but in practive this should not be a concern. 

Note this means that most of the time your service is running, it may be killed by the system if it is under heavy memory pressure. If the this happens the sytem will later try to resetart the service. An important consequence of this is that if you implement onStartCommand() to schedule work to be done asynchronously or in another thread, then you may want to use START_FLAG_REDELIVERY to have the system re-deliver an Intent for you so that it does not get lost if your service is killed while processing it. 

Other application components running in the same process as the service (such as an Activity) can, of course, increase the importance of the overall process byond just the importance of the service itself. 
