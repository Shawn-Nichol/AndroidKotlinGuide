# CheckBoxes
Checkboxes allow the user to select one or more options from a set, typically prseneted in a vertical list. 

To create each checkbox option, create a `CheckBox` in your layout. Because a set of checkbox options allows the user to select multiple items, each checkbox is managed separately and you must register a click listener for each one. 

A key class is the following
- `CheckBox`

## Responding to click Events 
When the user selects a checkbox, the checkBox object receives an on-click event. 
To define the click event handler for a checkbox, add the `android:onClick` attribute to the <CheckBox. eleemtn in your XML layout. THe value fo this attribute must be the name of the method you want to call in response to a click event. The activity hosting the layout must then implement the corresponding method. 

For example, here are a couple `CheckBox` objects in a list. 

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <CheckBox android:id="@+id/checkbox_meat"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/meat"
        android:onClick="onCheckboxClicked"/>
    <CheckBox android:id="@+id/checkbox_cheese"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/cheese"
        android:onClick="onCheckboxClicked"/>
</LinearLayout>
```

The `Activity` or `Fragment` that hosts the checkboxes.
```
fun onCheckboxClicked(view: View) {
    if (view is CheckBox) {
        val checked: Boolean = view.isChecked

        when (view.id) {
            R.id.checkbox_meat -> {
                if (checked) {
                    // Put some meat on the sandwich
                } else {
                    // Remove the meat
                }
            }
            R.id.checkbox_cheese -> {
                if (checked) {
                    // Cheese me
                } else {
                    // I'm lactose intolerant
                }
            }
            // TODO: Veggie sandwich
        }
    }
}
```

The method you declare in the `android:onClick` attribute must have a signature exactly. 
- Be Publicc
- Return void
- Define a View as its only parameter. 
