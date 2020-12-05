# Edit Text options

```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <EditTextPreference
        app:key="counting"
        app:title="Counting preference"/>

    <EditTextPreference
        app:key="number"
        app:title="Numbers only preference"/>
</PreferenceScreen>
```


## Update information in the preference screen. 
```
class EditTextPreferences : PreferenceFragmentCompat() {

    private val TAG = "EditTextPreferences"

    override fun onCreatePreferences(savedInstanceState: Bundle?, rootKey: String?) {
        setPreferencesFromResource(R.xml.preferences_edit_text, rootKey)
        Log.i(TAG, "onCreatePreferences")

        // Update summary
        val counting: EditTextPreference? = findPreference("counting")
        counting?.summaryProvider = Preference.SummaryProvider<EditTextPreference> { preference ->
            val text = preference.text
            if(TextUtils.isEmpty(text)) "Not Set"
            else "Length of saved value: ${text.length}"
        }

        // User can only enter a number input
        val number: EditTextPreference? = findPreference("number")
        number?.setOnBindEditTextListener { editText ->
            editText.inputType = InputType.TYPE_CLASS_NUMBER
        }

        // Handle click of preference.
        number?.setOnPreferenceClickListener {
            Log.i(TAG, "number onclick")
            true
        }
    }
}

```
