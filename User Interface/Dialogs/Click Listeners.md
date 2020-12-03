# Fragment/Activity
Even though the launchAlertDialog() method is isnde of a Fragment, the "host" for the AlertDialog is in the activity.



## Fragment
Add `setFragment()` to the `MyAlertDialog` object so the listener can commnicate with fragment. 
```
classs MyFragment : Fragment(), MyAlertDailog.MyAlertListener {
  
  ...
  
    override fun onDialogPostiveCLick(dialog: DialogFragment) {
        Log.i(TAG, "Listener returns a postive click")
    }

    override fun onDialogNegativeClick(dialog: DialogFragment) {
        Log.i(TAG, "Listener returns a negative click")
    }
    
    fun launchAlertDialog() {
      val dialog = MyAlertDialog()
      // Listener needs to know what fragment you are using. 
      dialog.setTargetFragment(this, 0)
      dialog.show(requireActivity().supportFragmentManager, "MyAlertDialog")
   }
}


class MyAlertDialog : DialogFragment() {
  internal lateinit var listener: MyAlerListener
  
  
    /*
     * The activity that creates an instance of this dialog fragment must implemnt this interface in order
     * to receive event callbacks. Each method passes the DialogFragment in case the host needs it.
     */
    interface MyAlertDialogListener{
        fun onDialogPostiveCLick(dialog: DialogFragment)
        fun onDialogNegativeClick(dialog: DialogFragment)
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)

        // Verify that the host activity implements the callback interface
        try {
            // Instantiate the NoticeDialogListener so events can be sent to the host.
            listener = context as MyAlertDialogListener
        } catch (e: ClassCastException) {
            // The activity doesn't implement the interface, throw exception
            throw ClassCastException((context.toString() + " must implement MyAlertDialogListener"))
        }
    }
    
    
        override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {

        return activity?.let {
            // Use the Builder class for convenient dialog contruction
            val builder = AlertDialog.Builder(it)
            builder.setMessage("My Dialog message")
                .setTitle("Dialog Title")
                .setPositiveButton("Positive button",
                    DialogInterface.OnClickListener{ dialog, id ->
                        Log.i(TAG, "onCreateDialog, postive button")
                        listener.onDialogPostiveCLick(this)

                    })
                .setNegativeButton("Negative ",
                    DialogInterface.OnClickListener { dialog, which ->
                        Log.i(TAG, "onCreateDialog, negative button")
                        listener.onDialogNegativeClick(this)


                })
            builder.create()
        } ?: throw IllegalStateException("Activity cannot be null")
    }
  
}

```

