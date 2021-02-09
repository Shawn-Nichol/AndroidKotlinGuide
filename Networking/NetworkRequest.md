Permission
```
// Accesss network checks the network state of the device. 
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
// Internet access the internet.
<uses-permission android:name="android.permisssion.INTERNET"/>
```

Network requests are not allowed on the app's main thread. also called the UI thread. If you block the main thread, android causes the app to throw an exception. 


`doAsync()` is part of the a domain-specific language (DSL) provided by the Kotlin llibrary, Anko. You can include Anko by including the following lin in app level build.gradle
```
implementation "org.jetbrains.anko:anko-common:0.10.8"
```
Anko provides a way to execute code on a separate worker thread and return the control flow back to the main thread by calling uiThread()

## check the network Availablitiy
Before you can make a network request, you have to be sure that there is a network available. 

### Confirm Network Connection

## Reading URL content
Open Reques



Glossary
ConnectivityManager
