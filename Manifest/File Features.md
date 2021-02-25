
## File features
The manifest file's root element requires an attribute for your app's package name. 

EX. the root <Manifest> element with the package name "com.example.myapp"
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
1. It applies this name as the namespace for your app's generated R class.

2 It uses this name to resolve any relative class names that are declared in the manifest file
Ex. An activity declared as <activity android:name=".ManinActivity"> is resolved to be "com.example.myapp.MainActivity"

As such, the name in the manifest's package attribute should always match your project's base package name where you keep your activites and other app code. Of course, you can have other sub-packages in your project, but those files must import the R class using the namespace from the package attribute. 

However, beware that, once the APK is compiled, the package attribute also represents your app's universally unique application ID. After the build tools perform the above tasks based on the package name, they replace the package value with the value given to the applicaitionID property in your project's build.gradle file (used in Android Studio projects). THis final value for the package attribute must be universally unique becuase it is the only guaranteed way to identify your app on the system and in Google Play. 

The distinction between the package name in the manifest and the applicctionID in the build.gradle file can be a bit confusing. But if you keep them the same, you have  nothing to worry about. 

