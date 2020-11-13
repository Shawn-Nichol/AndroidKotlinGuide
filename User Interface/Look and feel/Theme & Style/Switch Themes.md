
## How to change Themes well the app is running
Create to themes in the `res/values/styles.xml` file
```
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="android:forceDarkAllowed">true</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="android:windowTranslucentStatus">true</item>
        <!-- This allows the contextual action bar to cover the toolbar instead of pushing it down. -->
        <item name="windowActionModeOverlay">true</item>
    </style>

    <!-- Base application theme. -->
    <style name="AppTheme2" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="android:forceDarkAllowed">true</item>
        <item name="colorPrimary">@color/colorPrimaryTwo</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDarkTwo</item>
        <item name="colorAccent">@color/colorAccentTwo</item>
        <item name="android:windowTranslucentStatus">true</item>
        <!-- This allows the contextual action bar to cover the toolbar instead of pushing it down. -->
        <item name="windowActionModeOverlay">true</item>
    </style>
```

Save a value to identify the desired theme and recreate() the activity.
```
val private userTheme: Int = 1

override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return when(item.itemId) {
        ... 
        R.id.them_change -> {
            userTheme = if(userTheme == 1) 2
            else 1
            recreate()
            return true
        }
        else -> super.onOptionsItemSelected(item)
    }
}
```

When the activity reload you will need to set the theme in `onCreate()` before the `super()`.
```
override fun onCreate(savedInstanceState: Bundle?) {
    // needs to load before super so the correct theme will load.
    when (userTheme) {
        1 -> {
            setTheme(R.style.AppTheme)
            Log.i(TAG, "Load Theme 1")
        }
        2 -> {
            setTheme(R.style.AppTheme2)
            Log.i(TAG, "Load Theme 2")
        }
    }
    super.onCreate(savedInstanceState)
    ...
}
```
