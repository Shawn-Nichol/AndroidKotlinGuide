# Persistent state
There are generally two kinds of persistent state that an activity will deal with: shared document-like data and internal state such as user preferences. 

## Edit in place
For content provider data use "edit in place", any edits a user makes are effectively made without requring an additional confirmation step. Supporting this model is generally a simple matter of following two rules.

- When creating a new document, the backing database entry or file for it is created immediately. For example, if the user chooses to write a new email, a new entry for that email is created as soon as they start entering data, so that if they go to any other activity after that point this email will now appear in the list of drafts. 

- When an activity's onPause() is called, it should commit to the backing content provider or file any changes the user has made. This ensures that those changes will be seen by any other activity that is about to run. you will probably want to commit your data even more aggressively at key times during your activity's lifecycle: for example before starting a new activity, before finishing you own activity, when the use rswitches between input fields

This model is designed to prevent data loss when a user is navigating between activities, and allows the system to safely kill an activity at any time after it has been stopped. Note this implies that the user pressing back from your activity does not mean "cancel" it means to leave the activity with its current contents saved away. Canceling edits in an activity must be provied through some other mechanism, such as an explicit revert or undo option. 

The activity class also provides an API for managing internal persistent state associated with an activity. This can be used for example, to remember the user's preferred inital display in a calendar or the user's default home page in a web browser. 

The activity class also provides an API for managing internal persistent state associated with an activity. This can be used, for example to remember the user's preferred initial display in a calendar or the user's default home page in a web browser. 

Activity persistent state is manged with the method getPreferences(int), allowing you to retrieve and modify a set of name/value pairs associated with the activity. To use preferences that are shared across multiple application components, you can use the underlying Context#getSharedPreferences to retieve a preferences object stored under a specific name.

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

When naming you shared prefenece files, you should use a name that's uniquesly identifiable to your app. An easy way to do this is prefix the file name with your applicationID. For example "com.example.myapp.PREFERENCE_FILE_KEY"

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
