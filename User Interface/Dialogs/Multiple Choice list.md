# Multiple choice list
To add a list of multiple choice items
```
class MyAlertDialog : DialogFragment {
  override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
      return activity?.let {
          val selectedItems = ArrayList<Int>() // Where we track the selected items
          val builder = AlertDialog.Builder(it)
          // Set the dialog title
          builder.setTitle(R.string.pick_toppings)
                  // Specify the list array, the items to be selected by default (null for none),
                  // and the listener through which to receive callbacks when items are selected
                  .setMultiChoiceItems(R.array.toppings, null,
                          DialogInterface.OnMultiChoiceClickListener { dialog, which, isChecked ->
                              if (isChecked) {
                                  // If the user checked the item, add it to the selected items
                                  selectedItems.add(which)
                              } else if (selectedItems.contains(which)) {
                                  // Else, if the item is already in the array, remove it
                                  selectedItems.remove(Integer.valueOf(which))
                              }
                          })
                  // Set the action buttons
                  .setPositiveButton(R.string.ok,
                          DialogInterface.OnClickListener { dialog, id ->
                              // User clicked OK, so save the selectedItems results somewhere
                              // or return them to the component that opened the dialog
                              ...
                          })
                  .setNegativeButton(R.string.cancel,
                          DialogInterface.OnClickListener { dialog, id ->
                              ...
                          })

          builder.create()
      } ?: throw IllegalStateException("Activity cannot be null")
  }
}
```
