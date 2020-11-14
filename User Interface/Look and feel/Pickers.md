# Pickers
Android provides control for the user to pick a time or pick a date as ready-to-use dialogs. Each picker provides control for selectin each part of the time or date. using these pickers helps ensure that your users can pick a time or date that is valid, formatted correclty, and adjsuted to the user's locale. 

We recommend that you use `DialogFragment` to hsot each time or datepicker. The `DialogFragment` was first added to the platform in Android 3.0, if you r app suppports version of Android older thatn 3.0 even as low as Android1.6  you can use the `Dialogragment` class that's available in the support library for backward compatibility. 

Note: The code samples below show how to create dialogs for atime picker and date picker using the support libarry. Apis for DialgoFragment. if your app's minSkdVersion is 11 or higher, you can instead use the platform version of DialogFragemnt

key classes are the following
- DatePickerDialog
- TimePickerDialog

## Creating a Time Picker
To display a TimepickerDialog using `DialogFragment`, you need to define a fragment class that extends `DialogFragment` and return a `imePickerDialog` from the fragment's `onCreateDialgo() methods. 

## Extneding DialogFragemnt for a time Picker
