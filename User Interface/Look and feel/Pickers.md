# Pickers
Android provides control for the user to pick a time or pick a date. Each picker provides control for selection each part of the time or date. 

Use `DialogFragment` to host a picker. T

key classes are the following
- DatePickerDialog
- TimePickerDialog

## Creating a Time Picker
To display a TimepickerDialog using `DialogFragment`, you need to define a fragment class that extends `DialogFragment` and return a `imePickerDialog` from the fragment's `onCreateDialgo()`. 

## Time Picker
To define a `DialogFragment` for a `TimePickerDialog`, you must
- Define the `onCreateDialog()` method to return an instance of TimePickerDialog
- Implement the `TimePickerDialog.OnTimeSetListener` interface to receive a callback when the user sets the time. 

```
class TimePickerFragment : DialogFragment(), TimePickerDialog.OnTimeSetListener {

    override fun onCreateDialog(savedInstanceState: Bundle): Dialog {
        // Use the current time as the default values for the picker
        val c = Calendar.getInstance()
        val hour = c.get(Calendar.HOUR_OF_DAY)
        val minute = c.get(Calendar.MINUTE)

        // Create a new instance of TimePickerDialog and return it
        return TimePickerDialog(activity, this, hour, minute, DateFormat.is24HourFormat(activity))
    }

    override fun onTimeSet(view: TimePicker, hourOfDay: Int, minute: Int) {
        // Do something with the time chosen by the user
    }
}
```
Now you need an event that adds an instance of this fragment to your activity. 


## Show the time picker
Once you've defined a `DailogFragment` you can display the time picker by creating an instance of the `DialogFragment` and calling `Show()`.
```
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/pick_time"
    android:onClick="showTimePickerDialog" />
```

When the user clicks this button the system calls the following method. 
```
fun showTimePickerDialog(v: View) {
    TimePickerFragment().show(supportFragmentManager, "timePicker")
}
```

This method calls `show()` on  a new instance of the `DialogFragment` defined above. The `show()` method requires an instanace of `FragmentManager` and a unique tag name for the fragment. 


## Create a DatePicker
Creating a `DatePickerDialog` is just like creating a `TimePickerDialog`. The only difference is the dailog you create for the fragment. 

To display a `DatePickerDialog` using `DailogFragment`, you need to define a fragment class that extends `DailogFragment` and return a `DatePickerDialog` from the frament's `onCreateDialog()`

## Extending DialogFragment for a date Picker
To define a `DailogFragment` for a `DatePickerDailog` you must
- Define the `onCreateDialog()` to return an instance of `DatePickerDailog`
- Implement the `DatePickerDialog.OnDateSetListener` interface to receive a callback when the user sets the date. 

```
class DatePickerFragment : DialogFragment(), DatePickerDialog.OnDateSetListener {

    override fun onCreateDialog(savedInstanceState: Bundle): Dialog {
        // Use the current date as the default date in the picker
        val c = Calendar.getInstance()
        val year = c.get(Calendar.YEAR)
        val month = c.get(Calendar.MONTH)
        val day = c.get(Calendar.DAY_OF_MONTH)

        // Create a new instance of DatePickerDialog and return it
        return DatePickerDialog(activity, this, year, month, day)
    }

    override fun onDateSet(view: DatePicker, year: Int, month: Int, day: Int) {
        // Do something with the date chosen by the user
    }
}
```

Now all you need is an even that adds an instance of this grament to your activity. 

## Showing the date picker
Once you've defined a `DialogFragment` like the one shown above, you can display the date picker by creating an instance of the `DialogFragment` and calling `show()`.

```
<Button
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:text="@string/pick_date"
  android:onClick="showDatePickerDialog"
```

When the user click this method 
```
fun showDatePickerDialog(v: View) {
  val newFragment = DatePickerFragment()
  newFragment.show(SupportFragmentManager, "datePicker")
}
```
This method calls `show()` on a new instance of the `DialogFragment` defined above. The `show()` method requires an instance of `FragmentManager` and a unique tage name for the fragment. 

## Using pickers with autofill
Android 8 introduces the Autofill Framework, which allows users to save data that can be later used to fill out forms in differen tapps. Pickers can be useful in autofill scenarios by providing a UI that lets users change the value of a field that stores date or time data. For examlple, in a credit card. 

Becuase Pickers are dialogs, they aren't displayed in an activty along with other fields. To display the picker data when the picker isn't visible, you can use another view, such as an `EditText`, which can display the value when the picker isn't visible. 

An `EditText` object natively expects autoffill data of type `AUTOFILL_TYPE_TEXT`. In constrast, autofill services should save the data as `AUTOFILL_TYPE_DATE` to be able to createa n appropriate representation of it. To solve the inconsistency in types, it's recommended that ou create a cstom view that inherits fomr `EditText` and implements the methods required to correctly handle values of type `AUTOFILL_TYPE_DATE`. 

You should take the following steps to createa subclass of `EditText` that can hanlde values of type `AUTOFILL_TYPE_DATE`:
1. Create a class that inherits form `EditText`
2. Implement the `getAutofillType()` method, which should return `AUTOFILL_TYPE_DATE`.
3. impelemnt the `getAutofillValue()` method, which should return an `AutofillValue` object taht represents the date in milliseconds. To create the reeturn object, use the `forDate()` method to generate an `AutofillValue`
4. Implement the `autofill()`. This method provides the logic to handle the `AutofillValue` parametere, which is of type `AUTOFILL_TYPE_DATE`. To handle the parameter create a proper string representation of it, such as `mm/yyyy`. Use the string representation to set the text property of your view. 
5. Implement functionality that displays a picker when the user wants t edit the date in teh custom subclass of `EditText`. The view should update the text property with a stirng representation of the value that user selected on the picker. 

For an example of a subclass of `EditText` that handles `AUTOFILL_TYPE_DATE` values, see the Autofill Framework sample. 
