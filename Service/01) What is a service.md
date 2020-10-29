# What is a Service
A service is an application that can perform long-running operatttion in the background, it does not provide a UI. Once started, a service might continue to run for some time, even after the user switches to another application. A component can bind to a service to interact with it even perform Inter Process Communication (IPC). Ex. Services can handle network transaction, play music perform file I/O, interact with content providers. 

A service is not a separate process. The service object itself does not imply it is running inits own process; unless otherwise specified, it runs in the same process as the application it is part of. 

A service is not a thread it is not a measn itsefl to do work off of the main thread.

A service is a facility for the application to tell the system about something it wants to be doing in the background. This corresponds to calls to COntext.StartService, which ask the system to schedule work ofr the service, to be run until the serice or someone else explicitly stop it. Its an application  to expose some of its functionality to other applications. This corresponds to calls to Context.bindService(), which allows a long-standing connection ot be made to ther service in order to interact with it. 

There are three types of services

## Foreground Serivce
A service preforms an operation that is noticiable to the user. Ex, like an audio player. Foreground services must display a notification. Foreground services continue running even when the user isn't interacting with the app. 

## Background
A background  service performs an operation that isn't directly noticed by the user. Ex, an app used service to compact its storage. 


## Bound
A Service is bound when an application component binds to it by calling bindService().  A bound service offers a client-server interface taht allows components to interact with the service, send requests, receive results and even do so across process with IPC. A bound services runs only as long as another application componet is bound toit multiple components can bind to the service at once, but when all them unbinnd the service is destroyed. 

Service can also be started and allow binding. It's simply a matter of whether you implement a couple of callback methods onStartCommand() to all components to start it and  onBind to allow binding. 

Regardless of whether your service is started, bound or both, any application component can use the service in the same way that any componet can use activity by starting it with an intent. However you can declare the service as private in the mainfest file and block access from other application. 
