# AlertDialog

THe alert Dialog class allows you to build a variety of dialog designs and is often the only dialog class you'll need. 

1. Title
This is optiona and should be sued only when the content are is occupied by a detailed message. 

2. Content are , this can dispaly a message a list or other custom layout features.

3. action buttons, You can have up to three action buttons. 
- Postive Button, accept and continue with the the action. 
- Negative Button, cancel the action 
- Neutral Button, use this when you don't want to proceed. 


## Create a new class that extends DialogFragment
```
class MyAlertDialog : DialogFragment() {

  // Override the onCretedialog, to build the DialogWindow.
  override fun onCreateDialog( saveInstanceState: Bundle): Dialog {
    // Instantiate the code with its constructore. 
    return activity?.let {
      // Use the builder class to chain combine various Dialog methods. 
      
      val builder = AlertDialog.Builder(it
      builder.setMessage("My message")
         
         // Action buttons
         .setPostiveButton("Positive button", DialogInterface.OnClickListener { dialog, id -> 
            // run a postive action
          }
          .setNegaitveButton("Negative Button", DialogInterface.OnClickListener { dialog, id -> 
            // run a negative action/
          }
          .setNeutralButton("Cancel Button", DialogInterface.OnClickListener { dialog, id -> 
            // cancel the dialog. 
          }
          .create()
    }
  }
}
```

## Dismiss Dialog
You can perfomr certain actions when the dialog goews away, by implementing the `onDismiss()` method before clo


