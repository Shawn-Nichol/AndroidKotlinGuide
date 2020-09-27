If you have a realitively small collection of key-values that you'd like to save, you should use the SharedPreferences API. A sharedPreferences Object point to a file contining key-value pairs and provides simple methods to read and write them Each SharedPreferences file is managed by the framework and can be private or shared. 

Note: the SharedPreferences APIs are for reading and writing key-value pairs, and you should not confuse them with Preference APIs, which help you build a user interrface for your app settings aloghth they also use SharedPreferences to save the user's settings. For information about the Prefences APIs, see the setting developer guide


## Get a hanlde to shared preferecnes
You can create a new shared prefercne file or access an existing one by calling one of these methods. 

- getSharedpreferences(): Use this if you need multiple shared preference files identified by name, which you specify with the first parameter. You can call this from any context in your app

- getPreferences: Use this from an activity if you need to use only one shared preference file for the activity. Becuase this retrieves a default shared preference file that belongs to the activity, you don't need to supply a name. 

Ex. This code access the named sharedPrefences file and opens it uisng the prive mode so the file is accessible by only your app. 
```
val sharedPref = activity?.getSharedPreferences(getString(R.string.preference_file_key), Context.MODE_PRIVATE)
```

When naming your shared preference files, you should use a name that's uniquely identifiable toyour app. An easy way to do this prefix the file name with your application ID for example. "com.example.myapp.PREFERENCE_FILE_KEY"

Alternatively, if you need just one shared preference file for your activity, you can use getPReferences() 
```
val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE)
```

If you're using the sharedPreferences API to save app settings, you should instead use getDefaultSharedPreferences() to get the deefault shared prefernce file for your entire app. For more information see the setting developer guide.

## Write to a shared Preferences
To write to a shared preferences file, create a SharedPreferences.EDitor by calling edit() on your sharedPreference.
Note: You can edit shared preferences in  a more secure way by calling the edit() method on an EnryptedSharedPreferences object instead of on a sharedpReferences object. To learn more.

Pass the Keys and values you want to write with methods such as putInt() and putString() then call apply() or commit() to save the changes.


```
val sharedPref = activity?.getPReferences(Context.MODE_PRIVATE) ?: return
with (sharedPref.edit()) {
  putInt(getString(R.string.saved_high_score_key), newHighScore)
  commit()
}
```

apply() changes the in-memory SharedPreferences object immediately but writes the updates to disk asynchronously. Alternatively, you can use commit() to write the data to disk synchronously. But becuase commit() is synchronous, you should avoid calling if from your main thread becase it could pause your UI rendering

## Read from shared preferences
To retrieve values form a shared preference fils, call methods such as getInt() and getString(), providing the key for th value you want and optionally a default value to return if the key isn't present. 

```
val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE) ?: return
val defaultValue = resources.getInteger(R.ingetr.saved_high_score_default_key)
val highScore == sharedPref.getInt(getString(R.string.saved_high_Score_key),defaultValue)
```
