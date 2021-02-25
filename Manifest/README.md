# Manifest file
Every app progject must have an AndroidManifest.xml file at the root the project source set. The manifest file describes essential informataion about your app to the Android build tools, the Android operating system, and Google Play. 

The manifest files is required to declare the following
- The app's package name. The android build tools use this to determine the location of code entities when building your project. When packaging the app, the build tools replace this value with the application ID from the Gradle build files, which is used as the unique app identifier on the system and on Google play. 
- All the components of the app. Each component must define basic properties such as the name of its Kotlin class. It can also declare capabilities such as which device configurations it can handle, and intent filters that describe how the component can be started. 

- The permissions that the app needs in order to access protected parts of the system or other apps. It also declares any permissions that other apps must have if they want to access content from this app. 

- The hardware and software features the app requires, which affects which devices can install the app from Google Play. 


