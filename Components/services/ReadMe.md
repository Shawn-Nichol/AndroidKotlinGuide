A service is an application component that can peform long-running oepration in the background, It does not provide a user interface. Once started, a service might continue running for some time, even after the user switches to another application. Additionally, a component can bind to a service to interact with it even perform Inter Process Communication (IPC). For example, a service can handle network transactions, play music, perform file I/O, or interact with conten tprovider, all form the background. 

A service does not create its oown thread and does not run in a separate process unless you specify otherwise. You should run any blocking operations on a separte thread within the service to avoid application not responding (ANR)

There are three types of services. 

## Foreground service
A foreground service performs some Operations that is noticiable to the user. For example, an audio app would use a foreground service to play an audio track. Foreground services must display a Notfication. Foreground services continue running even when the user isn't interacting with the app.

## Background
A background Service performs an operation that isn't directly noticed by the user. For example if an app used service to compact its storage, that would usually bee a background service. 

## Bound
A service is bound when an application component bind s to it by calling bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, receive results and even do so across processes with IPC.  A bound service runs only as long as another application component is bound to it Multiple components can bind to the service at once, but when all of them unbind the service is destoroyed. 

Service can also be started and also allow binding. It's simply a amtter of whether you implement a couple of callbacks methods: onStartCommand() to all components to start it and onBind to allow binding. 

Regardless of whether you service is started, bound or both, any application component can use the service in the same way that any component can use anctivity by starting it with an intent. However you can declare the service as private in the manifest file and block access fromother applicaitons. 
