# Radio Buttons
Radio buttons allow the user to select one option from a set. 

To create each radio button option, create `RadioButton` in your layout. However, becuase radio buttons are mutally exclusive, you must group them together inside a `RadioGroup`. By grouping them together, the system ensures that only one radio button can be selected at a time. 

Key class are the following:
- RadioButton
- RadioGroup

## Responding to click Events 
When the user selects on of the  radio buttons, the corresponding `RadioButton` object receives an on-click event. 

To define the click event handler for a button, add the `android:onClick` attribute to the `<RadioBtton>` element. The value for this attribute must be the name of the method you want to call in respones to a click event. The `Activity/Fragment` hosting the layout must then implement the corresponding method. 

```
<?xml version="1.0" encoding="utf-8"?>
<RadioGroup xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">
    <RadioButton android:id="@+id/radio_pirates"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/pirates"
        android:onClick="onRadioButtonClicked"/>
    <RadioButton android:id="@+id/radio_ninjas"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/ninjas"
        android:onClick="onRadioButtonClicked"/>
</RadioGroup>
```

The following method handle the click event for both radio buttons. 

```
fun onRadioButtonClicked(view: View) {
    if (view is RadioButton) {
        // Is the button now checked?
        val checked = view.isChecked

        // Check which radio button was clicked
        when (view.getId()) {
            R.id.radio_pirates ->
                if (checked) {
                    // Pirates are the best
                }
            R.id.radio_ninjas ->
                if (checked) {
                    // Ninjas rule
                }
        }
    }
}
```

The method you declare in the `android:onClick` attribute must have a signature exactly as shown above
- Be public
- Return void
- Define a View as its only parameter. 
