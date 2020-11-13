# What is a Theme
A theme is a collection of attributes that's applied to the entire app, activity, or view hierarchy not just an individual view. When you apply a theme, every view in the app or activity applies each of the theme's attributes that it supports. Themes can also apply styles to non-view elements, such as the status bar and window background. 

Themes are declared in a style resource file in res/valus/ ususally named `styles.xml`

Themes and styles have many similarities, but they are used for different purposes. Themes and styles have the same basic structure as a key value pair which maps attributes to resources. 

Styles and themes are meant to work together. For example, you might have a style that specifies that one part of a button should be `colorPriamry,` and another part should be colorSecondary. The actual definitions of those colors is provided in the theme. When the device goes into nightmode, your app can switch from its "light" theme to its "dark" theme, changing the values for all those resources names. you don't need to change the styles, since the styles are using the semantic names and not specific color definitions. 


## Customize the defual theme. 
When you create a project with Andoid studio, it applies a material design theme to your app by default, as defined in your project's `styles.xml` file. This `AppTheme` style extends a theme from the support library and includes overrides for color attrbiutes that are used by tkey UI elements, such as the app bar and `FAB`. So you can quickly customize your app's color design by updaitn the provides colors. 

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <!-- Customize your theme here. -->
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
</style>
```

Style values are referenced to other color resources, defined in the project's `res/values/colors.xml` files. That's the file you should edit to change the colors. You can preview your colors with the `Material Color Tool`. This tool helps you pick colors from the material palette and preview how they'll look in an app. 

Update the values in `res/values/colors.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--   color for the app bar and other primary UI elements -->
    <color name="colorPrimary">#3F51B5</color>

    <!--   a darker variant of the primary color, used for
           the status bar (on Android 5.0+) and contextual app bars -->
    <color name="colorPrimaryDark">#303F9F</color>

    <!--   a secondary color for controls like checkboxes and text fields -->
    <color name="colorAccent">#FF4081</color>
</resources>
```

And then you can override what ever other styles you want. For exmaple, you can change the activity background color as follows. 
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    ...
    <item name="android:windowBackground">@color/activityBackground</item>
</style>
```

For a list of attrbiutes you can use in your theme, see the table of attrbitues at `R.styleable.Theme` and when adding styles for the views in your layout, you can also find attrbiutes by looking at the "XML attrbiutes" table in the view class references. For example, all views support `XML attrbiutes from the base view class.`

Most atttributes are applied to specific types of views, and some apply to all views. However, some theme attrbiutes listed at `R.styleable.Theme` apply to the activity window, not the views in the layout. For example,  windowBackground changes the window background and windowEnterTransition defines a transition animation to use when the aticity starts.

The Android Support Library also provides other attributes you can use to customize your theme, extend from `Theme.ApCompat`(such as the colorPrimary attribute shown above). Theses are best viewed in the library's attrs.xml file. 

There are also different themes available from the suport library that you might want to extend. The best place to see the available themes is the library's themes.xml file. 

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
