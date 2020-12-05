# Single Preference setup

## XML file
```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <ListPreference
        app:allowDividerBelow="true"
        android:defaultValue="1"
        // The entry values array as a resource
        app:entries="@array/item_array"
        // the array to bue used as values to save for preferences. 
        app:entryValues="@array/item_array"
        android:key="List"
        android:title="Single List"
        app:summary="A list where you can select a single item."
        />

</PreferenceScreen>
```


## Update summary of list
The onCreatePreference will be called when there is change in the prefence.

```
class ListPreferences : PreferenceFragmentCompat() {

    override fun onCreatePreferences(savedInstanceState: Bundle?, rootKey: String?) {
        setPreferencesFromResource(R.xml.preferences_list, rootKey)

        // Reference the List
        val listOne: ListPreference? = findPreference("List")
        // Update summary with the users selected option.
        listOne?.summaryProvider = Preference.SummaryProvider<ListPreference>{ preference ->
            val text = preference.value.toString()
            "List one, $text selected"
        }
    }
}
```
