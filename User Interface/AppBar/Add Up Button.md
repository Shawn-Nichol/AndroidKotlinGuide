## Add Up action
Up action will send the user back to the parenty activity 


## Declare a Parent Activity
To suport up functionality in an activity, you need to declare the activit's parent. You can do this in the app manifest, by setting an android:paretnActivityName attrbiute.

manifest
```
android:parentActivityName="com.example.myfirstapp.MainActivity"

```

## enable up button
```
    // Get a support ActionBar corresponding to this toolbar and enable the Up button
    supportActionBar?.setDisplayHomeAsUpEnabled(true)
```
