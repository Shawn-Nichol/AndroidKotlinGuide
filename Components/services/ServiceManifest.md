# Declaring Service in the Manifest File
You must delcare all services in your application maifest file. To declare a service, add a <service> element as a child of <application element. 

```
<manifest ...>
  <application ...> 
    <service android:name=".exampleService"/>
  </application>
<manifest>
```

There are other attributes that you can include in teh <service> element to define properties sucha s permission that are required to start the service and the process in which the service should run. The android: name attribute it specifies the clasas name of the service. After you publish your application leave this name unchaged toa void the risk of breaking code due to dependence on explicit intents to start or bind service. 


To ensure that your app is secure, always use an explicit intent when starting a SErvice adn don't declare intent filters for your services. Using an  implicit intent to start a service is a security hazard becuase you cannot be certain of the service that responds to the intent, tand the user cannont see which service starts. 

You can ensure that your service is avialbalbe o only your app by including the android: exproted attribute and settign it to false. This effectively stops other apps from starting your service, even when using an explicit intent. 

Note: 
User can see what services are running on their device. If they see a service that they don't recongize or trust, they can stop the service. In order to avoid having your service stopped accidentally by users, you need to add the adnroid description attribute to the <service. element in your app manifest. In the description, provided a short sendtence explaining what the service does an what benfits it provides. 
