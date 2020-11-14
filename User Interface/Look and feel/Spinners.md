# Spinners
Spinners provide a quick way to select one value from a set. In the default state, a spinner shows its currently selected value. Touching the spinner displays a dropdown menu with all other available values, from which the user can select a new one. 

You can add a spinner to you layout with the Spinner object. you should usually do so in the XML  layout with a `<Spinner>` element. 

```
<Spinner
    android:id="@+id/planets_spinner"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

To populate the spinner with a list of choices, you then need to specify a `SpinnerAdapter` in your Activity or Fragment

Key classes
- Spinner 
- SpinnerAdapter
- AdapterView.OnItemSelectedListenre

## Populate the Spinner with User Choices
The choice you provide for the SpinnerAdapter, such as an ArrayAdapter if the choices are aailabel in ana array or a `CursorAdapter` if the choices are available form a database query. 

For instance, if the availabel choices for your spinner are pre-determined, you can provide them with a string array defined in a string resource file

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="planets_array">
        <item>Mercury</item>
        <item>Venus</item>
        <item>Earth</item>
        <item>Mars</item>
        <item>Jupiter</item>
        <item>Saturn</item>
        <item>Uranus</item>
        <item>Neptune</item>
    </string-array>
</resources>
```

Load the array in to the spinner. 
```
val spinner: Spinner = findViewById(R.id.spinner)
// Create an ArrayAdapter using the string array and a default spinner layout
ArrayAdapter.createFromResource(
        this,
        R.array.planets_array,
        android.R.layout.simple_spinner_item
).also { adapter ->
    // Specify the layout to use when the list of choices appears
    adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
    // Apply the adapter to the spinner
    spinner.adapter = adapter
}
```

To `createFromResource()` method allows you to create an `ArrayAdapter` from the strin garray. The thrid argument for this method is a layout resource that defines how the selected choice appears in teh spinner control. The simple_spinner_item layout is provided by the platform and is the default layout you should use unless you'd like to define your own layout for the spinner's appearance. 

You should then call `setDropDownViewResource(int)` to specify the layout the adapter should use to display the list of spinner choices (`simple_spinner_dropdown_item` is another standard layout defined by the platform).

Call setAdapter() to apply the adapter to your `Spinner`. 

## Responding to User Selection
When the user selects an item from the drop-down, the `Spinner` object receives an on-item-sleected event. 

To define th eselction even thandler for a spinner, implement the `AdapterView.OnitemSelectedListener` interface and the corresponding `onItemSelected(0` callback method. For exmaple, here's an implementation of the interface in an Activity. 

```
class SpinnerActivity : Activity(), AdapterView.OnItemSelectedListener {

    override fun onItemSelected(parent: AdapterView<*>, view: View?, pos: Int, id: Long) {
        // An item was selected. You can retrieve the selected item using
        // parent.getItemAtPosition(pos)
    }

    override fun onNothingSelected(parent: AdapterView<*>) {
        // Another interface callback
    }
}
```

The `AdapterView.OnItemSelecteListener requires teh onItemSelected() and `onNothingSelected() callback methods. 

Then you need to specify the interface implementation by calling setonitemSelectedListenere()
```
val spinner: Spinner = findViewById(R.id.spinner)
spinner.onItemSelectedListener = this
```

When you implement the `AdapterView.OnItemSelectedListener` interface with your `Activity` or `Fragment`, you can pass this as the interface instance. 
