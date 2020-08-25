# Started Services

A started service is one that another component starts by calling startSErvice(), which results in a call to the service's onStartedCommand().

When a service is started, it has a lifecycle that's indpenendent of the component that started it. The service can run in the background indefinitely, even if the component that started it is destroyed. As such the service should stop itself when its job is complete by calling stopSelf(), or another component can stop it by calling stopSErvice(). 

An application component such as an activity can start the service by calling startSEervice() and passing an intent that specifies the service and includes any data for the service to use. The service reciieves this intent in the onStartCommand(). 

A services runs in thesame process as the application which it is declared and in the main thread of the application by default. If your service performs intensive or blockign operations while the user interacts with an activity fromthe sam application, the service slows down activity performance. To avoid impacting application performance start a new thread inside the service. 
You can use JobInentservices as a replacement for Intentseervices. 
