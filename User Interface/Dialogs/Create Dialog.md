# Creating a Dialog

Create a new class that extends DialogFragment
```
class MyAlertDialog : DialogFragment() {

  // Override the onCretedialog, to build the DialogWindow.
  override fun onCreateDialog( saveInstanceState: Bundle): Dialog {
    return activity?.let {
      // Use the builder class for convenitent dialog construction
      val builder = AlertDialog.Builder(it)
      builder.setMessage("My message")
         
         // set action buttons you can have up to three, a Postive Negitave and cancel
         .setPostiveButton("Positive button", DialogInterface.OnClickListener { dialog, id -> 
            // run a postive action
          }
          .setNegaitveButton("Negative Button", DialogInterface.OnClickListener { dialog, id -> 
            // run a negative action/
          }
          .setCancelButton("Cancel Button", DialogInterface.OnClickListener { dialog, id -> 
            // cancel the dialog. 
          }
          .create()
    }
  }
}
```


