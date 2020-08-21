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

