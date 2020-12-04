# Use saved values
Preference Data Storage
By default, a `Preference` uses `SharedPreferences` to save values. The `SharedPreferences` API allows for reading and writing simple key-value pairs from a file that is saved across appliction sessions. The Preference Library uses a private `SharedPreferences` instance so that only youra pplication can access it. 

As an example, assume the followign SwitchPreferncecompat.
```
<SwitchPreferenceCompat
        app:key="notifications"
        app:title="Enable message notifications"/
```

When a user toggles this switch to the On state,the `SharedPreferences` file is updated with a key-value pair of "notifications" : "True". Note that  the key used is the same as the key set for the `Preference`. 


## Preference DataStore
While the Preference library persists data with `SharedPreferences` by default, `SharedPReferences` aren't always an ideal sooution. For example, if your application requires a user to sign in, you might wwant to persist application settings in the cloud so that the settings are reflected across othere devices and platforms. Similarly, if your application has configuration options that are ddevice-specific, each user on the device would have separate settings, making `SharedPreferences` a less-than-idea solution. 


## Reading Preference values. 
To retrieve the `SharedPreferences` object that is being used, call `PreferenceManager.getDefaultSharedPreferences(0`. This method works from anywehre in your application. For example, given an `EditTextPreference` with a key of "signature":
```
<EditTextPreference
        app:key="signature"
        app:title="Your signature"/>
```

The saved value for this Preference can be retrieved globally as follows. 
```
val sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this /* Activity context */)
val name = sharedPreferences.getString("signature", "")
```

## Listen for changes to Preference values
To listen for chagnes to Preference values, you can choose between two interfaces
- Preference.OnPReferenceChangeListener
- SharedPReferences.OnSharedPreferenceChangegListener

The table below show how the two interfaces differ 

onPreferenceChangeListener
- Set on a per-Preference basis
- callced when a pReference is about to change its saved value. This includes if the pending value isthe same as the curretnly saved value. 
- Only called throughte Preference library. A separate part of the application could change the saved value. 
- Called before the pending value is saved. 
- Called when using SharedPReferences or a PreferenceDataStore. 

OnSharedPrefernceChangeListener
- Applies to all Preferences
- Called only when the value saved for a Prefernce has changed. 
- Called whenever the value saved has changed, even it it is from a separate part of the application. 
- CAlled after the value has already been saved. 
- Called only when using SharedPreferences. 

## OnPreferenceChangeListner
Implementing an OnPReferenceChangeListener allows you to listen for when the value of a PRefernce is about to change. From there, you can validate if this change should occur. For example, the code below shows how to listen for a change to the value of an EditTextPreference with a key of name. 
```
override fun onPreferenceChange(preference: Preference, newValue: Any): Boolean {
    Log.e("preference", "Pending Preference value is: $newValue")
    return true
}
```

Next set the listener directly with setOnPreferenceChangeListener(). 
```
prefernce.onPreferenceChangeListener = ...

```

## OnSharedPreferenceChangeListener
When persisting PReference values vsuing SharedPRefernces, you can also use a SharedPRefernces.OnSharedPreferences, you can also use a sharedPRefernces.OnsharedPReferncChangeListneer to listen for changes. This allows you to listen for whent he values saved by your preference are chagned, such as when syncing settings with a server. The example below shows how to listen for when the value of an EditTextPreference with a key of name changes. 

```
override fun onSharedPreferenceChanged(sharedPreferences: SharedPreferences, key: String) {
    if (key == "signature") {
        Log.i(TAG, "Preference value was updated to: " + sharedPreferences.getString(key, ""))
    }
}

```
You must also register the listener via registerOnsharedPReferenceChangedListener(). 
```
preferenceManager.sharedPreferences.registerOnSharedPreferenceChangeListener(...)
```

For proper lifecycle management in the Activity or Fragment,you should register and unregsiter the listener in the onResume and onPause(0 callbacks, 

```
override fun onResume() {
    super.onResume()
    preferenceManager.sharedPreferences.registerOnSharedPreferenceChangeListener(this)
}

override fun onPause() {
    super.onPause()
    preferenceManager.sharedPreferences.unregisterOnSharedPreferenceChangeListener(this)
}

```

## Using a cusom data store
While persisting PReference objects using `SharedPreferences` is recommended, you can also cuse a custom data store. A custom data store can be useful if your application persists vaules to a database or if values are device-specific

## Impelemtn the data store. 
To implement a custom data store, first creata clas that extends PReferenceDataStore. 
```
class DataStore : PreferenceDataStore() {
    override fun putString(key: String, value: String?) {
        // Save the value somewhere
    }

    override fun getString(key: String, defValue: String?): String? {
        // Retrieve the value
    }
}

```

Be sure to run any time-consuming operations off the main thread to avoid blocking the user interface> Since it is possible for the Fragment or Activity containing the data store to be destroyed while persisting a value, you should serialize tthe data so you don't lose any of during a configuration chagne. 

## enable the data store.
AFter you have implemented your data store, you must set the new data store in `onCreatePreferences() ` so that `preference` objects persist values with the data store instead of using the default `SharedPreferences. A data store can be enabled for each `preference` or for the entire hierarchy. 

To eanble a custom data store for a specific Preference, call setPReferenceDataStore() on the Preference,a s shown in teh example below. 
```
val preference: Preference? = findPreference("key")
preference?.preferenceDataStore = dataStore
```


A data store that is set for a specifi preference overrides any data store that is set for the corresponding hierarchy. In smost cases,you should set a data store for thw whole hierarchy 

Note: IF you set a data store for a Preference after teh Preference is attached to thehierarchy, the initial value for the Preference is not propagated again. 


