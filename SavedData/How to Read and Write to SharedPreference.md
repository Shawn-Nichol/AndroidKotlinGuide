# How Read and Write to sharedPreference

1) create a Hanlder for sharedPreference
```
var sharedPref: SharedPreferences = requireActivity.getSharedPreferences("file_name", default_value)
```

2) Read SharedPreference
```
sharedPref.getBoolean("key_one", default_value)
```

3) Write to SharedPreference file
```
with.sharedPref.edit()) {
  putBoolean("key_one", saved_value)
  commit()
}
```

## Puttin it all together
```
lateinit var sharedPref: SharedPreferences

override fun onAttach(context: Context) {
  ...
  // you can also use requireActivity inplace of context. 
  sharedPref = context.getSharedPreference("file_name", Context.MODE_PRIVATE)
}


override fun onCreate(savedInstanceState: Bundle?) {
  ...
  
 // First argument takes the string paramaeter for the key and second argument is the a default value. 
 var myData = sharedPref.getBoolean("key_one", false)
}

override fun onStop() {
  ...
  with(sharedPref.edit()) {
    putBoolean("key_one", saved_value)
    commit()
  }
}
```
