# Resource values
Some attributes have values that are displayed to users, such as the title for an activity or your app icon. The value for these attributes might differ based on the user's language or other device configurations (such as to provide a different icon size based on the device's pixel density), so the values should be set from a a resource or theme, instead of hard-coded into the manifest file. The actual value can then change based on alternative resources that you provide for different device configurations. 

Resources are expressed as values with the following format
```
"@[package:]type/name
```

You can omit the package name if the resource is provided by your app including if it is provided by a library dependency, becuase library resources are merged into yours). The only other valid package name is android, when you want to use a reosurce from the Android framework. 

The type is a type of resource, such as string or drawable, and the name is the name that identifies the specific resource. 
```
<activity android:icon="@drawable/smallPic"...>
```

<action> adds an action to an intent filter
<activity> Declares an activity component.
<activity-alias> Declares an alias for an activity
<category> Adds a category name to an intent filter.
<compatible-screens> Specifies each screen configurations with which the application is compatible. 
  <data> Adds a data specification to an intent filter. 
<grant-uri-permission> Specifies the subsests of app data that the parent content provider has permission to access. 
<instrumentation> Declares an Instrumentation class that enables you to monitor an application's interaction with the system.
<intent-filter> Specifies the types of intents that an activity,service or broadcast receiver can respond to. 
<manifest> The root eleemtn of the AndroidManifest.xml
<meta-data> A name-value pair for an item of additional, arbitrary data that can be supplied ot the parent component.
<path-permission> Defines the path and required permissions for a specific subset of data within a content provider. 
<permission> Declares a security permission that can be used to limit access to specific components or features of this or other applications. 
<permission-group> Declares a name for a logical grouping of related permissions
<permission-tree> Declares the base name for a tree of permissions
<provider> Declares a content provider component
<receiver> Declares a broadcast receiver component
<service> Declares a service component.
<supports-gl-texture> Declares a service component.
<supports-screens> Declares the screen sizes your app supports and enables screen compatibility mode for screens larger than what your app supports. 
<uses-configuration> Indicates specific input features the application requires
<uses-feature> declares a single hardware or software feature that is used by the application. 
<uses-library> Specifies a shared library that the application must be linked against.
<uses_permission> Specifies a system permission that the user must grant in order for the app to operate correctly. 
<Uses-permission-sdk-23> Specifies that an app wants a particular permission, but only if the app is installed on a device running Android 6.0 (API level 23)
<uses-sdk> Lets you express an application's compatibility with one or more versioin sof the Android platform, by means of an API level integer. 
  
  ## Multiple values. 
If more than one value can be specified, the element is almost always repeated, rather than multiple values being listed within a single element. For example, an intent filter can list several actions: 
```
<intnet-filter ...> 
  <action android:name="android.intent.action.EDIT"/>
  <action android:name="android.intent.action.INSERT"/>
  <action android: name="android.intent.action.DELETE"/>
</intent-filter>
```
