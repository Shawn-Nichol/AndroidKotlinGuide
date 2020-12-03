# Create a cusom Dialog

## Creat the XML file for the layout
```
?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">
    <ImageView
        android:src="@drawable/ic_walk"
        android:layout_width="match_parent"
        android:layout_height="64dp"
        android:scaleType="center"
        android:background="#FFFFBB33"
        android:contentDescription="@string/app_name" />
    <EditText
        android:id="@+id/username"
        android:inputType="textEmailAddress"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:layout_marginLeft="4dp"
        android:layout_marginRight="4dp"
        android:layout_marginBottom="4dp"
        android:hint="User Name" />
    <EditText
        android:id="@+id/password"
        android:inputType="textPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="4dp"
        android:layout_marginLeft="4dp"
        android:layout_marginRight="4dp"
        android:layout_marginBottom="16dp"
        android:fontFamily="sans-serif"
        android:hint="Password"/>
</LinearLayout>
```

## Load the custom_dialog layout into the Dialog window. 
```
class CustomDialog : DialogFragment() {

    override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        return activity?.let {
            val builder = AlertDialog.Builder(it)
            // Get the layout inflater
            val inflater = requireActivity().layoutInflater

            builder.apply {
                // Inflate and set the layout for the dialog
                // Pass null as the parent view becuase its going in the dialog layout.
                setView(inflater.inflate(R.layout.custom_dialog, null))
                setPositiveButton("Sign In",
                DialogInterface.OnClickListener { dialog, which ->
                    // Sign in user
                })

                setNegativeButton("Cancel",
                DialogInterface.OnClickListener { dialog, which ->
                    getDialog()?.cancel()
                })
            }



            builder.create()
        } ?: throw IllegalStateException("Activity cannot be null")
    }
}
```
