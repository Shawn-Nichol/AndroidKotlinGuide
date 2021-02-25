## Permissions
Android apps must request permission to access sensitive user data or certain system features. Each permission is identified by a unique label. 

Ex, an app that needs to send SMS messages must have the following line in the manifest:
```
<manifest ... >
    <uses-permission android:name="android.permission.SEND_SMS"/>
    ...
</manifest>
```

Beginning with Android 6.0 (API level 23), the user can approve or reject some app permissions at runtime. But no matter which Android version you app supports, you must declare all permission requesets with a <use_permission> element in the manifest. If the permission is granted, the app is able to use the protected features. If not, its attempts to acces those features fail. 

Your app can also protect its own components with permissions. It can use any of the permissions that are defined by Android, as listed in Android.manifest.permissions, or a permission that's declared in another app. You app can also define its own permissions. A new permission is declared with the <permission> element. 
