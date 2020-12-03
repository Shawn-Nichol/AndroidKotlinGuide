Single list
Because the list apperas in the dialogs content area, the dialog cannot show both a message and a list, To specify the items for the list, call setItems(), passing an array. Alertantively, you can speicfy a list using setAdapter() this allows you to back the list with dynamic data using ListAdapter. 


```
class MyAlertDialog : DialogFragment {

  override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
      return activity?.let {
          val builder = AlertDialog.Builder(it)
          builder.setTitle(R.string.pick_color)
                  .setItems(R.array.colors_array,
                          DialogInterface.OnClickListener { dialog, which ->
                              // The 'which' argument contains the index position
                              // of the selected item
                          })
          builder.create()
      } ?: throw IllegalStateException("Activity cannot be null")
  }
}

```
