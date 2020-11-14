# Radio Buttons
Radio buttons allow the user to select one option from a set. You should use radio buttons for optional sets that are mutally exclusive if you thing that the user needs  to see all available options side-by-side. If it's not necessary to show all options side-byside, use a spinner instead. 

To create each readio button option, creata `RadioButton` in your layout. However, becuase radio buttons are mutally exclusive, you must group them toether inside a `RadioGroup`. By grouping them together, the system ensures that only one radio button can be selected at a time. 

Key class are the following:
- RadioButton
- RadioGroup

## Responding to click Events 
When the user selects on of the  radio ubttons, the corresponding `RadioButton` object receives an on-click event. 

To define the click event hadnler for a button, add the `android:onClick` attribute to the `<RadioBtton>` elemetn in you XML layout. THe value fo this attribute must be the name ofthe method you want to call in respones to a click eevent. The `Activity hosting the layout must then implement the corresponding method. 

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

Within the Activity that hosts this layout, the following method handle the click event for both radio buttons. 

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

The mthod you declare in the `android:onClick` attribute must have a signature exactly as shown above
- Be public
- Return void
- Define a View as its only parameter. 
