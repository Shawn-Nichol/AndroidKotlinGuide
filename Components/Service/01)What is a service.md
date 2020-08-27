# What is a Service
A service is an application that can perform long-running operatttion in the background, it does not provide a UI. Once started, a service might continue to run for some time, even after the user switches to another application. A component can bind to a service to interact with it even perform Inter Process Communication (IPC). Ex. Services can handle network transaction, play music perform file I/O, interact with content providers. 

There are three types of services

## Foreground Serivce
A service preforms an operation that is noticiable to the user. Ex, like an audio player. Foreground services must display a notification. Foreground services continue running even when the user isn't interacting with the app. 

## Background
A background  service performs an operation that isn't directly noticed by the user. Ex, an app used service to compact its storage. 


## Bound
A Service is bound when an application component binds to it by calling bindService().  A bound service offers a client-server interface taht allows components to interact with the service, send requests, receive results and even do so across process with IPC. A bound services runs only as long as another application componet is bound toit multiple components can bind to the service at once, but when all them unbinnd the service is destroyed. 

Service can also be started and allow binding. It's simply a matter of whether you implement a couple of callback methods onStartCommand() to all components to start it and  onBind to allow binding. 

Regardless of whether your service is started, bound or both, any application component can use the service in the same way that any componet can use activity by starting it with an intent. However you can declare the service as private in the mainfest file and block access from other application. 
