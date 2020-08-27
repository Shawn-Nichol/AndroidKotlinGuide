# Create a bond service. 

A bound service is one that allows applicationi components too bind to it by calling bindService() to create a long-standing connection. It generally doesn't allow components to start it by calling startService().

Create a bound service when you want to interact with the service from activites and other components in your application or to expose some of your applications functionality to other application through interprocess compunication(IPC).

To create a bound service, implement the onBind() callback method to return an IBinder that defines the interface fro commmunication with the service. Other application components can then call bindSErvice() to retrieve the interface and begin calling methods on the service. The service lives only to serve the application component that is bound to it, so when there are no components bound to the service the system destroys it. You don't need to stop a servie in the same way that you must when the service isstarted through onStartCommand()

Multiple clients can bind to the service simultanesously. When a client is done interacting with the service, it calls unbindSErvice() to unbind. When there are no client boudn to the service, the system destorys the service. 

There are multiple ways to implement a boudn serice, and the implementation is more complicated than a started service. Fore thesee reasonees, the bound service dicussion appears in a separate docuemnt about bound services. 

## Sending Notification to the user
When 
