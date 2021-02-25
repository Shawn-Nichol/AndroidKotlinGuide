# Get a handle
You can create a new shared preference file or access an existing one by calling 
- getSharedPreferences() - Use this if you need  multiple shared preference files identified by name, which you specify with the first parameter. You can call this from any context in your app. 

- getPreferences() use this from an Activity if you need to use only one shared prefferences file for the activity. Becuase this retrieves a default shared preference file that belongs to the activity, you don't need to suppply a name. 

Ex, the following code accesses the shared preferences file that's identified by the resource string R.string.preference_file_key and opens it using the private mode so the file is accessible by only your app
```
val sharedPref = activity?.getSharedPreferences(getString(R.string.preference_file_key), Context.MODE_PRIVATE)
```

When naming you shared prefenece files, you should use a unique name that is identifiable to your app. An easy way to do this is prefix the file name with your applicationID. For example "com.example.myapp.PREFERENCE_FILE_KEY"
Alternatively if you need just one shared preference file for your activity, you can use the getPReferences method
```
val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE)

```

If you're using the SharedPreferences API to save app settings, you should instead getDefaultSharedPreferences() to get the default shared prefence file for your entire app
