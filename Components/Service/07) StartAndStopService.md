# Start a service
You can start a service forman activity or other application components by passying an intent to startService() or startForegroundService(). The android system calls the service onStartCommand() method and passes it the Intetn, which specifies which  service to start. 

An activity can start a service using an explicit intet with startService()
```
Intent(this, MyService::class.java).also { intent -> 
  startService(intent)
}
```

The startService() method returns immediately, and the Android system calls the service's onStartCommand() methdo. If the service isn't already running, the system first calls onCreate(), and then it calls onStartCommand(). 

If the service doesn't also provide binding, the intent that is delivered with startService() is the only mode of communication between the application component and the service. However, if you want the service to send a result back, the client that starts the service can create a PendingIntent for a broacast(with getBroadcast()) and deliver service. However, if you want the service to send a result back, the client that start the service can create a Pending Intent for a broaddcast(with getBroad cast()) and deliver it to the service in teh Intent that starts the srvice. The service can then use the broadcast to deliver a result. Multiple requests to start the service result in multiple corresponding calls to the servic'es onStartCommand(). However, only one request to stop the service(withstopSelf() or stopService()) is required to stop it. 

# Stop service
A started service must manage its own lifecycle. That is the system doesn't stop or destroy the service unless it must recover system memory and the service continues to run after onStartCommand() returns. The service must stop itself by calling stopSelf(), or another component can stop it by calling stop Service().

Once requested to stop with stopSelf() or stopService(), the system destroyes the service as soon as possible. 

If your services hanldes multipe requests to onStartCommand() concurrently, you snhouldn't stop the service when you're done processing a start request, as you might have received a new start request stopping at the end o fthe first request would terminate the second one). To avoid this problem, you can use stopSelft(int) to ensure that your request to stop the service is always based on teh most recent start request. That is, when  you call stopSelf(int), you pass the ID of the start request (the startID delivered to onStartCommand() to which your stop request corresponds. Then, if the services  receives a new start request before you are able to call stoSelf(int), the ID doesn't match and the serice doesn't stop. 
