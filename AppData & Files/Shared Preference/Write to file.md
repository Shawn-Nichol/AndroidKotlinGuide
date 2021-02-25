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


