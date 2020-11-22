# Persistent state
There are generally two kinds of persistent state that an activity will deal with: shared document-like data and internal state such as user preferences. 

## Edit in place
For content provider data use "edit in place", any edits a user makes are effectively made without requring an additional confirmation step. Supporting this model is generally a simple matter of following two rules.

- When creating a new document, the backing database entry or file for it is created immediately. For example, if the user chooses to write a new email, a new entry for that email is created as soon as they start entering data, so that if they go to any other activity after that point this email will now appear in the list of drafts. 

- When an activity's onPause() is called, it should commit to the backing content provider or file any changes the user has made. This ensures that those changes will be seen by any other activity that is about to run. you will probably want to commit your data even more aggressively at key times during your activity's lifecycle: for example before starting a new activity, before finishing you own activity, when the use rswitches between input fields



## How to Create 
1) Create Handler for sharedPreference
```
private lateinit var sharedPref: SharedPrefernces
```

2) Initilize sharedPref handler in onCreate(), and retrieve KeyValue pair
```
override fun onCreate(...) {
    ...
    sharedPref = activity?.getSharedPreferences(getString.preference_file_key), Context.MODE_PRIVATE) ?: return
    
    // retieve Item
    myData = sharedPref.getInt(MY_KEY, 0)
}
```

3) Save Data in onPause()
```
override fun onPause() {
    ...
    with(sharedPref.edit()) {
        putInt(MY_KEY, myData)
        apply() // you can also you use commit see notes below.
    }

}

```
Note: 
apply: changes the in-memory SharedPreffence object immediately but writes the updates to the disk Asyncronously. 
Commit: will write synchronously and could pause the UI well writing to the SharedPrefenece file. 
