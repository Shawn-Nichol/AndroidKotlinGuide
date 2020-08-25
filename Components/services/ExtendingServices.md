# Extending the Service class
You can extend the service class to handle each incoming intent. Here's how a basic implementationmight look:

onStartCommand() method must return an integer. The integer is a value that desribes how the system should continue the service in the event that the system kills it. the return value from onStartCommand() must be one of the following. 

## START_NOT_STICKY
If the system kills the service after onSTartCommnad() returns, do not recreate the service unless there are pending intents to deliver. This is the safest option to avoid running your service when not necessary and when your application can simply restart any unfinished jobs. 

## START_STICKY
if the system kills the service after onStartCommand() retuns, recreate the service and call onStartCommand(), but do not redilver the last intent. Instead the system calls onStartCommand() with a null intent unless there are pending intents to start the service. In that case, those intents are delivered. This is suitable for media players that are not executing commands but are running indefinitely and waiting for a job. 

## START_REDELIVERE_INTENT
If thes sytem kills the service after onStartCommand() returns, recreate the service and call onStartCommand() with the last intent that was delivered to the service. Any pending intents are delivered in turn. This is suitable for services that are actively performing a job that should be immediately resumed suuch as a downloading a file. 
