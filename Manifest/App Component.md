## App Component
For each component that you use, you must declare corresponding XML element in the manifest file. 
- <activity> fore each subclass of Activity
- <service> for each subclass of service.
- <receiver> for each subclass of BroadcastReceiver
- <provider> For each subclass of ContentProvider. 

If you subclass any of thesse component without declaring it in the manifest file,the system cannot start it. The name of your subclass must be specified with the name attribute, using the full package desgination. 

Ex, an Activity subclass can be declared as follows:
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
