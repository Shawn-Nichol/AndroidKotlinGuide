# Organize Settings
 

## Preference categories
If you have several related Prefernces ona single screen, you can group them with a PrefernceCategoyr. A PreferenceCategory displays a categroy title and visually spearates the category. 

To define a PreferfenceCategory in XML, wrap the Preference tags with a PreferenceCategoryas shown

```
<PreferenceScreen
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <PreferenceCategory
        app:key="notifications_category"
        app:title="Notifications">

        <SwitchPreferenceCompat
            app:key="notifications"
            app:title="Enable message notifications"/>

    </PreferenceCategory>

    <PreferenceCategory
        app:key="help_category"
        app:title="Help">

        <Preference
            app:key="feedback"
            app:summary="Report technical issues or suggest new features"
            app:title="Send feedback"/>

    </PreferenceCategory>

</PreferenceScreen>
```

## Split your hierarchy into mutiples screens
Preference Screen allows you to separte preferences into different screens keeping a cleaner more organized look. 



### 1) Create Preference XML file
xml/preference_screen_two
```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <SwitchPreferenceCompat
        app:key="Switch"
        app:title="Switch"/>
</PreferenceScreen>
```

### 2) Create a new Preference screen
Create a new class, that extends PreferenceCompatFragment and overrides `onCreatePreferences`


onCreatePreferences
Called during onCreate(Bundle) to supply the preferences for this fragment. Subclasses are expected to call setPreferenceScreen(PreferenceScreen) either directly or via helper methods such as addPreferencesFromResource(int)
@savedInstanceState: Bundle, if the fragment is being re-created from a previous saved state this is the state. 
@rootKey: String, If non-null, this prefernce fragment should be rooted at the PrefernceScreen with this key. 

```
class ScreenTwo : PreferenceCompatFragment() {

    override fun onCreatePreference(savedInstanceState: Bundle?, rootKey: String?) {
        sestPreferenceFromResource(R.xml.preference_screen_two, rootKey)
    
    }
}
```

### 3) Add Link to New Preference screen. 
```

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Preference
        // set the fragment to the package address of the new screen.
        app:fragment="com.example.screen2"
        app:key="UI"
        app:summary="UI summary"
        app:title="UI Screen" />

</PreferenceScreen>
```

### 3) Add `PreferenceFragmentCompat.OnPreferenceStartFramentCallback` to the MainActivity. 
```
class MainActivity : AppCompatActivity(), PreferenceFragmentCompat.OnPreferenceStartFragmentCallback {
```

### 4) Override PreferenceStartFragment
Called when the user clicked on a prefernce that has a fragment calass asssoicated with it. The implmenetaiton shoul dinstantiate and switch to an instance of the given fragment
@caller: PreferenceFragmentCompate, THe fragment requesting navigation
@pref: True, if the fragment creation has been handled. 
```
override fun onPreferenceStartFragment(
    caller: PreferenceFragmentCompat?,
    pref: Preference?
): Boolean {
    Log.i(TAG, "onPreferenceStartFragment, pref: $pref, Caller: $caller")

    when(pref?.key) {
        "ScreenOne" -> navController.navigate(R.id.dest_screenOne)
        "ScreenTwo" -> navController.navigate(R.id.dest_screenTwo)
        "ScreenThree" -> navController.navigate(R.id.dest_screenThree)
    }
    return true
}
```


