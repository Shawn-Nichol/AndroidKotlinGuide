# What is Shared Preferences
SharedPreference is a small collection of key-values pairs stored in a file, each shared preference file is managed by the framework and can be private or shared. There are two methods of geting a handle of a sharedPreference. 

- getSharedPReferences(): Used if you need save key-values to multiple files. Names should be unique to the app. 
```
val sharedPref = activity?.getSharedPreferences(getString(R.string.preference_file_key), Context.MODE_PRIVATE)
```
- getPreferences(): Used if you only need one file for the app. 
