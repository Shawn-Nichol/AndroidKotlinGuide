# Dark Theme

Dark Theme has many benefits
- Can reduce power usage by a significant amount.
- Improves visibility for users with low vision and those who are senstitive to bright light.
- Makes it easier for anyone to use a device in a low-light enviroment. 

Dark theme applies to both Android system UI and apps running on the device. 

There are three ways to enable Dark theme in Android 10 
1. Us the system setting (Setting->Display->Theme) to enable dark theme.
2. Use the Quick settings title to switch themes from the notfication tray.
3. On Pixel devices, selecting the battery saver mode enbles Drak theme at the same time. Other OEMs may or may not support this behavior. 

## Supporting Dark theme in your app. 
In order to support Dark theme, you must set your app's theme (usually found in `res/values/styles.xml` to inherit from a `DayNight` theme
```
<style name="AppTheme" parent="Theme.AppCompat.DayNight">
```

You can also use MaterialComponents' dark theming
```
<style name="AppThem" parent="Theme.MaterialCOmponents.DayNight">
```

This ties the app's main theme to the system controlled night mode flags and gives the app a default Dark them (when it is enabled).

## Themes and Styles. 
Your themes and styles should avoid hard-coded colors or icons intended for use under a light theme. you should use theme attrbutes or night-qualified resources instead. 

Here are the two most improtant theme attrbiutes to know about
- `?android:attr/textColorPrimary` this is a general purpose text color. It is near-black in Light theme and near-whitein Dark themes. it contains a disabled state. 
- `?attr/colorControlNormal` A general-purpose icon color. It conntains a disabled state. 

We recommend using `Material Design Components`, since its color theming system (suuch as the theme attrbitues `?attr/colorSurface` and `?attr/colorOnSurvase`) provies easy access to suitable colors. Of course, you can customize these attrbiutes in your theme. 

## Changing themes in-app
Your app can let the user choose between themes. The recommend options are 
- Light
- Dark
- System Default

Each of the options map directly to one of the AppCompat.DayNight modes
- Light - Mode_Night_NO
- Dark - MODE_NIGHT_YES
- System default - `MODE_NIGHT_FOLLOW_SYSTEM`

To swicth the theme, call `AppCompatDelegate.setDefaltNightMode()`.

Note: Starting with App Compat V 1.1.0, setDefaultNightMode() automatically recreates any started activites. 

## Force Dark
Android 10 provides Force Dark, a Feature for developers to quickly implement a Dark theme without explicitly setting a DayNight theme as described above. 

Force Dark analyzes each view of your light-themed app, and applies a dark theme automatically before it is drawn to the screen. Some developers use a mix of Force Dark and native implementation to cut down on the amount of time needed to impelment Dark theme. 

Apps must opt-in to Force Dark by setting android:forceDarkAllowed="true" in the activity's theme. This attribute is set on all of the system and androidX provided light themes, such as Theme. Material light. When you use Force Dark, you should make sure to test your app thoroughly and exlude views as needed. If your app uses a dark theme, Force dark will not be applied. Similarly, if your app's theme inherits from a DayNight Theme, Force Dark will not be applied, due to the automatic switching 

## Disabling Force Dark on a view
Force Dark can be controlled on specific views with the anddroid:forceDarkAllowed layout attrbiute or with `setForceDarkAllowed()`

## Best Practies
Notification and widgets 
For UI surfaces that you display on the device but do not directly control, it is improtant to make sure that any views you use reflect the host app's theme. Two good exampes are notification and launcher widgets

### Notfications 
Use the system-provided notification templates (such as MessaginingStyle). This means that the system is responsible for ensuring the correct view styling is applied. 

### Widgets and custom notifications views. 
For launcher widgets, or if your app uses custom notification content views, it is improtant to make sure you test the content on both the light and dark themes. 

Commmon pitfalls to llok out for
- Assuming that the background color is always light
- Hardcoding text colors. 
- Setting a hardcoded background color while using the default text color
- Using a drawable icon which is a static color

In all of these cases, use appropriate theme attrbiutes instead of hardcoded colors. 

## Launch screens
If your app has a custom launch screen it may need to be mdofified so that it refelcts the selected theme. 

Remove any hardcoded colors, for example any background colors pointing may be white. Use the `?android:attr/colorBackground` theme attrbites instead.

Note that dark-themed `android:windowBackground` drawables only work on Android Q. 

## Configurations changes

When the app's theme changes (either throughte the system setting or AppCompat) it triggers a uiMode configuration chagne. This means that activites will be automatically  recreated. 

In some cases you might want an app to handle the configuration chagne. For example, you might want to delay a configurationo chagne becuase a video is playing. 

An app can handle the implementation fo Dark theme itself by ddeclaring that each Activity can handle the uiMode configuration chagne

```
<activity
    android:name=".MyActivity"
    android:configChanges="uiMode" />
```
When an Activity declares it hanldes configration changes, its `onConfigurationChagned()` method will be calle dwhen ther eis a theme change. 

```
val currentNightMode = configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
when (currentNightMode) {
    Configuration.UI_MODE_NIGHT_NO -> {} // Night mode is not active, we're using the light theme
    Configuration.UI_MODE_NIGHT_YES -> {} // Night mode is active, we're using dark theme
}
```

