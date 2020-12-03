# Organize Settings
Large complex settings screens can make it difficult for a user to find a specific setting they wourld like to change. The preference library offers the following ways to better organize your settings screens. 

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
If you have a large number of Preferences or distinct categories, you can display them on  separate screens. Each screen should be a PreferenceFragmentCompat with its own separate hierarchy. Preference on your initial screen can then link to subscreens that contain related Preferences. 

To link screens with a PRefernce, you can declare an app:fragment in XML, or you ca use PRefernce.setFragment(). Set the full package name ofhe PrefenceFragmentCompat that should be launched when th PRefernce is tapped as shown below. 
```
<Preference
        app:fragment="com.example.SyncFragment"
        .../>
```

When a user taps a Preference with an assocaited fragment, the interface mtehod PrefrenceFragmentCompat.OnPReferncestartFragmentCallback.onPrefere ceStartFragment() is called. This method is where you should handle dispaly the new screen and should be implement in the surrounding Activity. 

Note: If you dont implement onPRefernceStartFragment(), a allback implementation is used instead. whil this works in most cases, we strongly recommend implemnting this mehtod so yo can fully configure transitions between fragments objects and update the titel dispalye din you Activity toolbar if applicable. 

class MyActivity : AppCompatActivity(),
    PreferenceFragmentCompat.OnPreferenceStartFragmentCallback {

    ...

    override fun onPreferenceStartFragment(caller: PreferenceFragmentCompat, pref: Preference): Boolean {
        // Instantiate the new Fragment
        val args = pref.extras
        val fragment = supportFragmentManager.fragmentFactory.instantiate(
                classLoader,
                pref.fragment)
        fragment.arguments = args
        fragment.setTargetFragment(caller, 0)
        // Replace the existing Fragment with the new Fragment
        supportFragmentManager.beginTransaction()
                .replace(R.id.settings_container, fragment)
                .addToBackStack(null)
                .commit()
        return true
    }
}

## PreferenceScreens
Declaring nested hierarchies within the samle XML resource using a nested <PrefernceScreen> is no longer supported. You should use nested Fragmet objects instead. 

## Use separate Activites
Alernatively, if you need to heavily custoize each scree, or i fyou want fully activity transitions between screens, you can use a separate Activity for each PreferenceFragmentcompat. By doing this, you can fully customize each Activity and its corresponding settgins screen. For most applications this  is not recommend and you should use fragmetns as previousl described. 
