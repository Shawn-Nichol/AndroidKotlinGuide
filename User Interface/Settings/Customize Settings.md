Custom Settings
To access an individual prefence, such as when getting or setting a PRefence value, use PReferenceFragmentCompat.findPreference(). THi smethod searches the entire hierarchy for a Preference with the given key. 
```
<EditTextPreference
        app:key="signature"
        app:title="Your signature"/>
```

Retrieve this prefernce 
```
override fun onCreatePreferences(savedInstanceState: Bundle?, rootKey: String?) {
    setPreferencesFromResource(R.xml.preferences, rootKey)
    val signaturePreference: EditTextPreference? = findPreference("signature")
    // do something with this preference
}
```

## Control Prefernce visibility
You can control which prefernces are visible o the user when they navigate to a settgins screen. 

 For example, if  a particular Prefernce is meaningufl only when a corresponding freature is eanbled, you might want to hide that prefernce when the feature is diabled. 
 
 To show a prefernce only when a condition i smet, first set the Preference visibility to false in the XML, as shown i
 ```
 <EditTextPreference
        app:key="signature"
        app:title="Your signature"
        app:isPreferenceVisible="false"/>
 ```
 
 Dynamically update summaries 
 A PRefence that persists data should dispaly the current value in its summar to help the user better understand the current state of the Prefence. For examlple an EditTextPreference should show the curretnly saved text value, and a listPreference should hso wth e currently selected list entry. You might also have prefernces that need to update their summary basedd on internal or externall app state a preference that dispalys a version number, for example. This can be done using a SummarProvider
 
 ## use a  Simply summarProvider
 ListPreference and EditTextPreference includ esimple SummaryProvider implementations that automtically display the saved PReference value as the summary. If no value has been savd they display Not Set. 
 
 You can enable these implementations from XML by setting app: useSimpleSummaryProvider="true"
 Alternatively, in code you can use ListPReference.SimpleSummaryProvider.getiNtance() and EditTextPRefenece. SimpleSummarProvider.getInstancee() to get the simple SummaryProvider instance and then set it on the PReference, as shown in the following example. 
 
 ```
 listPreference.summaryProvider = ListPreference.SimpleSummaryProvider.getInstance()
editTextPreference.summaryProvider = EditTextPreference.SimpleSummaryProvider.getInstance()
 ```
 
 ## Update custom SummarProvider
 You can creat your own SummarProvider and override provideSummar() to customize the summary whenever it is requested by the Preference. For example, the EditTextPreference below dispalys the length of its saved valu eas the summary
```
<EditTextPreference
        app:key="counting"
        app:title="Counting preference"/>
```
In onCreatePreferences(), acreate a new SummarProvider, and override provideSummar() to return the summar to be displayed. 
```
val countingPreference: EditTextPreference? = findPreference("counting")

countingPreference?.summaryProvider = SummaryProvider<EditTextPreference> { preference ->
    val text = preference.text
    if (TextUtils.isEmpty(text)) {
        "Not set"
    } else {
        "Length of saved value: " + text.length
    }
}
```

The preference summary now dispalys the length of the saved value or Not set when  no saved value exists. 

## Customize an EditTextPReference dialog
