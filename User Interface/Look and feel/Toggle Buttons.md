# What is a toggle Button
A toggle button allows the user to change a setting between two states. 

You can add a basic toggle button to your layout with the `ToggleButton` object. Android 4.0 introduces another kind of toggle button called a switch that provides a slider control, which you cna add with a Switch object. SwitchCompat is a version of the Switch widget which runs on a devices back to API 7. 

If you need to change a button's state yourself, you can use the `CompoundButton.setChecked()` or `CompoundButton.toggle()`

Key class
- ToggleButton
- Switch
- SwitchCompat
- CompoundButton

## Responding to Button Presses
To detec when the user activates the button or switch, create an `CompoundButton.OnCheckedChangeListener` object and assign it to the button by calling `setOncheckChangeListener`

```
val toggle: ToggleButton = findViewById(R.id.togglebutton)
toggle.setOnCheckedChangeListener { _, isChecked ->
    if (isChecked) {
        // The toggle is enabled
    } else {
        // The toggle is disabled
    }
}
```
