# How Read and Write to sharedPreference

1) create a Hanlder for sharedPreference
```
var sharedPref: SharedPreferences
val myBoolean = false

override fun onCreate(savedInstanceState: Bundle) {
  sharedPref = this.getSharedPreference("use file name as string", Context.MODE_PRIVAE)
  
}
```

2) Read SharedPreference
```
val KEY_ONE = "mySavedBoolean"
...
sharedPref.getBoolean(KEY_ONE, default_value)
```

3) Write to SharedPreference file
```
with(sharedPref.edit()) {
  putBoolean("key_one", saved_value)
  commit()
}
```

## Puttin it all together in a fragment
```
lateinit var sharedPref: SharedPreferences
val KEY_ONE = "mySavedBoolean"


override fun onAttach(context: Context) {
  ...
  // you can also use requireActivity inplace of context. 
  sharedPref = context.getSharedPreference("file_name", Context.MODE_PRIVATE)
}


override fun onCreate(savedInstanceState: Bundle?) {
  ...
  
 // First argument takes the string paramaeter for the key and second argument is the a default value. 
 var myData = sharedPref.getBoolean(KEY_ONE, false)
}

override fun onStop() {
  ...
  with(sharedPref.edit()) {
    putBoolean(KEY_ONE, saved_value)
    commit()
  }
}
```
