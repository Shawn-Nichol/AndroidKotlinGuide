## Device Compatibility
The manifest file is also where you can declare what types of hardware or software features your app requires, and thus which types of devices your app is compatible with. Google Play store does not allow your app to be installed on devices that don't provide the features or system version that your app requires. 

There are several manifest tags that define which devices your app is compatible with. Tee following are just a couple of the most common tags. 
<uses-feature>
The  <uses-feature elements allows you to declare hardware and software features your app needs. 
                   
Ex, if your app cannot achieve basic functionality on a device without a compass sensor, you can declare th compass senosr as required with the follwoing manifest tag:

```
<manifest ... >
    <uses-feature android:name="android.hardware.sensor.compass"
                  android:required="true" />
    ...
</manifest>
```

Each successive platform version often adds new APIs not available in the previos version. To indicate the mimimum version with which your app is compatible, your manifest must include the <uses-sdk> tag and its minSdkVersion attribute. 

Attributes in the <uses-sdk> element are overridden by corresponding properties in the build.gradle file. 
  
```
android {
  defaultConfig {
    applicationId 'com.example.myapp'

    // Defines the minimum API level required to run the app.
    minSdkVersion 15

    // Specifies the API level used to test the app.
    targetSdkVersion 28

  }
}
```
