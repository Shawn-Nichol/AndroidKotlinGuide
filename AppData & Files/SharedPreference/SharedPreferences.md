# Shared Preferences

## Save key-value data 
If you have a relatively small collection of key-values that you'd like to save, you should use SharedPreferences API. A A shared Preference objec points to a file containing key-value pairs and provides simple methods to read and write them. Each SharedPreferences file is managed by the framework and can be private or shared. 

## Get a handle
You can create a new shared preference file or access an existing one by calling one of these methods
- getSharedPreferences() - Use this if you need  multiple shared preference files identified by name, which you specify with the first parameter. You can call this from any context in your app. 

- getPreferences() use this from an Activity if you need to use only one shared prefferences file for the activity. Becuase this retrieves a default shared preference file that belongs to the activity, you don't need to suppply a name. 

For example the following code accesses the shared preferences file that's identified by the resource string R.string.preference_file_key and opens it using the private mode so the file is accessible by only your app
```
val sharedPref = activity?.getSharedPreferences(getString(R.string.preference_file_key), Context.MODE_PRIVATE)
```

When naming you shared prefenece files, you should use a unique name that is identifiable to your app. An easy way to do this is prefix the file name with your applicationID. For example "com.example.myapp.PREFERENCE_FILE_KEY"
Alternatively if you need just one shared preference file for your activity, you can use the getPReferences method
```
val sharedPref = activity?.getPreferences(Context.MODE_PRIVATE)

```

If you're using the SharedPreferences API to save app settings, you should instead getDefaultSharedPreferences() to get eh default shared prefence file for your entire app


## Write to shared preferences
To write to a shared preference file, create a sharedPreferences.Editor by calling edit on your sharedPReferences

Note you can edit shared prefereences in a more secure way by calling the edit() method on an EncryptedSharedPreferences boejct instead of on a sharedPRefereences object. To learn more 

Pass the keys and values you want to write with methods such as putInt() and putString()
Then call apply() or commit() to save chagnes

## Read From shared preferences
To Retrieve values from a shared preferences file ,call methods ush as getInt() na d getString, providing the key for the value you want, and optionallly a default value to return if the key isn't present



example with one shared preference file
```
class MainActivity : AppCompatActivity() {

    // Preference KEY
    val MY_KEY = "MainAcitivitySavedText"

    // Create a handler, with lateinit because context is required. 
    lateinit var sharedPref: SharedPreferences
    
    override fun onCreate(savedInstanceState: Bundle?) {
        ..
        sharedPref = getPreferences(Context.MODE_PRIVATE)
        
        // Read data from shared preference file, default value needs to be passed incase there is no key to read. 
        val saveText = sharedPref.getString(MY_KEY, "No value")
    }

    override fun onPause() {
        ...
        // Edit the preferences for the file
        with(sharedPref.edit()) {
            putString(MY_KEY, "the text i want to save")
            // Saves the data to the file. 
            commit()
        }
    }
}

```


example where you name the preference file and can have multiple preference files. 
```
class MainActivity : AppCompatActivity() {

    // Preference KEY
    val MY_KEY = "MainAcitivitySavedText"

    // Create a handler, with lateinit because context is required. 
    lateinit var sharedPref: SharedPreferences
    
    override fun onCreate(savedInstanceState: Bundle?) {
        ..
        sharedPref = getSharedPreferences("this is my prefence file", Context.MODE_PRIVATE)
        
        // Read data from shared preference file, default value needs to be passed incase there is no key to read. 
        val saveText = sharedPref.getString(MY_KEY, "No value")
    }

    override fun onPause() {
        ...
        // Edit the preferences for the file
        with(sharedPref.edit()) {
            putString(MY_KEY, "the text i want to save")
            // Saves the data to the file. 
            commit()
        }
    }
}

```


