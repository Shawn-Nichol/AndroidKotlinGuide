# Force Dark
Force dark is a feature that allows developers to quickly implement a Dark theme without explicitly setting a Day/Night theme. Force Dark analyzes each view of your light-themed app, and applies a dark theme automtically before it is drawn to the screen. You can use a mix of Force Dark and native impelmemtnaition to cut down on the amount of time needed to implement Dark Theme. 

Apps must opt-in to `Force Dark` by setting `android:forceDarkAllowed = true" in teh activity's theme. THis attribute is set on all the system and androidX provided light themes, such as Theme.Material light. Make sure to test thorougly and exclude views as needed. If the app uses dark theme, Force dark will not be applied. Similarly , if your app's theme inherits from DayNight Theme, Force Dark will not be applied due to the automitc swithicng. 

## Disable For dark on a view
`ForceDark` can be controlled on specific views with the `android:forceDarkAllowed laouot attrbiute or with `setForceDarkAllowed()`



How load dark theme

XML Theme
```
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <item name="android:forceDarkAllowed">true</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

```

Button 
```
        if(selectTheme) {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO)
        } else {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES)
        }
```
