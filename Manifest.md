# What is a Manifest file
Every app progject must have an AndroidManifest.xml file at the root the project source set. The manifest file describes essential informataion about your app to the Android build tools, the Android operating system, and Google Play. 

Among many other things, the manifest files is required to declare the following
- The app's package name, which usually matches your codes' namespace. The android build tools use this to determine the location of code entities when building your project. When packagingin the app, the build tools replace this value with the application ID from the Grradle build files, which is used as the unique app identifier on the system and on Google play. 
- Teh omponents of the app, which include all activites, services, broadcast receivers ,and content providers. Each component must define basic properties such as the name of its Kotlin or Java class. It can also declare capabilities such as which device configurations it can handle, and intent filters that describe how the component can be started. 

- The permissions that the app needs in order to access protected parts of the system or other apps. It also declares any permissions that other apps must have if they want to access content from this app. 

- The hardware and software features the app requires, which affects which devices can install the app from Google Play. 

If you're using Android Studio to build your app, the manifest file is created for you, and most of the essential manifest eleemtns are added as you build your app. 

## File features
The following section sdecribes how some of the most important characteristics of your app are reflected in the manifest file. 

The manifest file's root element requires an attribute for your app's package name (usually matching your project directory struvture-the Java namespace). 

For example, the following snippet shows the root <Manifest> eleemtn with the package name "com.example.myapp"
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xlmlns:android="http://schemas.android.com/apk/res/android:
  package="com.example.myapp"
  android:versionCode="1"
  android:versionname="1.0"
  ...
</manifest>
```

While building your app into the final application package (APK), the Android build tools use the package attribute for two things. 
- It applies this name as the namespace for your app's generated R.java class (used to access your app resources).
Example: with the above manifest, the R class is created at com.example.myapp.R

- It uses this name to resolve any relative class names that are declared in the manifest file
Example: With the above manifest, an activity declared as <activity android:name=".ManinActivity"> is resolved to be "com.example.myapp.MainActivity"

As such, the name in the manifest's package attribute should always amtch your project's base package name where you kep your activites and other app code. Of course, you can have other sub-packages in your project, but those files must import the R.java class using the namespace from the package attribute. 

However, beware that, once the APK is compiled, the package attribute also represents your app's universally unique application ID. After the build tools perform the above tasks based on the package name, they replace the package value with the value given to the applicaitionID property in your project's build.gradle file (used in Android Studio projects). THis final value for the package attribute must be universally unique becuase it is the only guaranteed way to identify your app on the systme and in Google Play. 

The distinction between the package name in the manifest and the applicctionID in the build.gradle file can be a bit confusing. But if you keep them the same, you have  nothing to worry about. 

Whoever, if you decide to make your code's namespace something differen from the application Id from the build file, be sure you fully understnadt implications of setting the application ID. That page explains how you can safely adjust he manifest's package naem independent of the build file's applicationId, and chagne the application ID for different buidl configurations. 

## App Component
For each app component that you create in your app, you must declare corresponding XML element in the manifest file. 
- <activity> fore each subclass of Activity
- <service> for each subclass of service.
- <receiver> for each subclass of BroadcastReceiver
- <provider> For each subclass of ContentProvider. 

If you subclass any of thesse component without declaring it in the manifest file,the system cannot start it. The name of your subclass must be specified with the name attribute, using the full package desgination. For example, an Activity subclass can be declared as follows:
```
<manifest ...> 
  <application ...> 
    <activity android:name="com.exmaple.myapp.MainActivity" ... >
    </activity>
  </application>
</manifest>
```

However, if the first character in the name value is a period, the app's package name from <manifest> element's package attribute) is prefixed to the name. For example, the following activity name is resolveed to "com.example.myapp.MainActivity"
```
<manifest package="com.example.myapp" ... >
    <application ... >
        <activity android:name=".MainActivity" ... >
            ...
        </activity>
    </application>
</manifest>
```
If you have app components that reside in sub-packages (such as in com.example.myapp.purchases), the name value must add the missing sub-package names (such as ".purchases.Payactivity") or use the fully-qualified package name. 

### Intent filters
App activites, services, and braodcast receivers are activated by intents. An intent is a message defined by an Intent object that describes an action to perform, including the data to be acted upon, the cateogry of omponent that should perfrom the action, and other instructions

When an app issues an intent to the system, the system locates an app component that can handle the intent based on intent filter declaration sin each app's manifest file. THe system launches an instance of the matching component and passes the Intent object to the component. If more than one app can handle the intent, then the user can selecct which app to use. 

An app component can have any number of intent filters (defined iwth the <intent-filter> element), eahc one describing a different capability of that component. 

Icons and labels
A number of manifest elements have icon and label attributes for displaying a small icon and text label, respectively, to users for the corrresponding app componenet. In every case, the icon and label that are set in a paren telement beomce the default icon  andlable value for all child eleemtns. For example, the icon and label that are set in the <application> element are the default icon and label for each of the app's components (such as all activites). 

The icon and label that are set in a omponent's <intent-filter> are shown to the user whenever that component is presented as an option to fulfilll an intent. By deffault, this icon i sinheritedfrom which ever icon is declared for the parent component(either the <activity> or <application> elemednt), but you might want to change the icon for an inent filter if tit provides a unique action that you'd like to better indicate in the choose dialog. 

## Permissions Android apps must request permission to access sensitive user data or certain system features. Each permission is identified by a unique label. For example, an app that needs to send SMS messages must have the following line in the manifest:
```
<manifest ... >
    <uses-permission android:name="android.permission.SEND_SMS"/>
    ...
</manifest>
```

Beginning with Android 6.0 (API level 23), the user can approve or reject some app permissions at runtime.But no matter which Android version you app supports, you must declare all permission requesets whit a <use_permission> element in the manifest. If the permission is granted, the app is able to use the protected features. If not, its attempts to acces those features fail. 

Your app can also protect its own components with permissions. It can use any of the permissions that are defined by Android, as listed in Android.manifest.permissions, or a permission that's declared in another app. You app can also define its own permissions. A new permission is declared with the <permission> eleemnt. 

## Device compatibility
The manifest file is also where you can declare what types of hardware or software features your app requires, and thus , which types of devices your app is compatible with .Google Play store does not allow your app to be installed on devices that don't provide the features or system version that your app requires. 

There are several manifest tags that define which devices your app is compatible with. THe following are jsut a couple of the most common tags. 
<uses-feature>
The  <uses-feature elements allows you to declare hardware and software features your app needs. For example, if your app cannot achieve basic functionality on a device without a compass sensor, you can declare th compass senosr as required with the follwoing manifest tag:

```
<manifest ... >
    <uses-feature android:name="android.hardware.sensor.compass"
                  android:required="true" />
    ...
</manifest>
```

Each successive platform version often adds new APIs not available in the previos version. To indicate the mimimum version with which your app is compatible, your manifest must include the <uses-sdk> tag and its minSdkVersion attribute. 

However, beware that attributes in the <uses-skd> eleemnt are overridden by corresponding properties in teh build.gradle file. So if you're using android Studio, you must specify the minSdkVersion and targetSdkVersion values there. 
```
android {
  defaultConfig {
    applicationId 'com.example.myapp'

    // Defines the minimum API level required to run the app.
    minSdkVersion 15

    // Specifies the API level used to test the app.
    targetSdkVersion 28

    ...
  }
}
```
