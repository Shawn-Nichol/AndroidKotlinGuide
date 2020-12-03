# Settings
Settings allow the users to change the functionality and behavior of an applicatoin. Setting s canaffect background behavior such as how often the application synchronizes data with the cloud, or they can be mroe wide-reaching, such as changing the contents and presentation of the user interafce. 

The recommened way ot integrate user configurable settings into your application is to use the Android Preference Library. This library manages the user interface and interface and interacts with storage so that you define only the individual settings that the user can configure. The library comes with a Material theme that provies a consisten user experinence across devices and OS version 

## Gettings started a preferenc eis the basic building bock of the PRefernce Library. A settghin screen contains a Preference hierracy. You can define this hierarchy as an XML resource, or you can build a heirarchy in code. 

## Create a hierarchy
Hierarchy contains individual preferences: a Switch PreferenceCompat that allows a user to toggle a settign on or off, and a basic PRefernce with no widget. 

ehn buildinga hierarchy, each Preference should have a unique key. 

## inflate the hierarcy
TO infalte a hierarchy from an XMl attribute creata PreferenceFragmentCompat, override onCreatePrefreence() and provide the XML resource to infalte as shown in teh exxample below
```
class MySettingsFragment : PreferenceFragmentCompat() {
    override fun onCreatePreferences(savedInstanceState: Bundle?, rootKey: String?) {
        setPreferencesFromResource(R.xml.preferences, rootKey)
    }
}
```
This fragment can be added to the activity as any other fragment. 
