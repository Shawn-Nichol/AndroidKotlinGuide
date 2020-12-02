A dialog is a small window that prompts the user to make a decision on enter additional information. A dialgo doe snot fill the screen and is normally used for modal events that require uers to take an action before they can proceed. 

The Dialog class is the base class for dialogs, but you should avoid instantiating Dialog directly instead use one of the following subclassed
AlertDailog
A dialog tha can show a title, up to three buttons a list of selectable items or a custom layout. 
DatePickerDialog or Time Picker dialog
A idialog with pre-defined  UI that allows the user to select a date or time. 

These classes define the style and structure for your dialog, but you should use a DialogFragment as a container for your dialog. The `DialogFragmetn` class provides all teh controls you need to create your dialog and manage its appearance, instead of calling methods on the `Dialog object`. 

Using `DialogFragment` to manage the dialog ensures that it correctly handles lifecycle events such as when the user presses the back button or rotates the screen. The `DialogFragment` class also allows you to reuse the dialog's UI as an embeddable componet in a larger UI, just like a traditional `Fragment` such as hwen you want the dialog UI to appear differently on large and smallscreens)

Note: Because the DialogFragmetn class was originally added with Android 3.0 this docuemtn descibes how to use the Dialogragment class that's provided with support libarry by adding this lbrarry to your app, you can use DialogFragemnt and a variety of other APIs on devices running Android 1.6 or higher. If the minimum version your app supports is API level 11 or higher, then you can use the fame work veresion of DialogFragment but be aware that the links in this docuemtn are the support library 

## Creating a Dialog
You can accomplish a wide variety of dialog designs including custom layouts and those described int he Dialogs design guide by extending DialogFragment and creating a AlertDialog in the onCreaeteDialog() callback 
```

class FireMissilesDialogFragment : DialogFragment() {

    override fun onCreateDialog(savedInstanceState: Bundle): Dialog {
        return activity?.let {
            // Use the Builder class for convenient dialog construction
            val builder = AlertDialog.Builder(it)
            builder.setMessage(R.string.dialog_fire_missiles)
                    .setPositiveButton(R.string.fire,
                            DialogInterface.OnClickListener { dialog, id ->
                                // FIRE ZE MISSILES!
                            })
                    .setNegativeButton(R.string.cancel,
                            DialogInterface.OnClickListener { dialog, id ->
                                // User cancelled the dialog
                            })
            // Create the AlertDialog object and return it
            builder.create()
        } ?: throw IllegalStateException("Activity cannot be null")
    }
}
```

Now when you createa n instance of this class and call show() on the at object the dialog appears. 

Depending on how cmoplex your dialog is, you can implement a variety of other callback methods in the DialogFragment, including all the basic fragment lifecycle methods. 


## Building an AlertDialog
The alertDialog class allows you to build a variety of dialog designs and is often the only idalog calass you'll need. 

1 Title
This is optional and should be used only when the content area is occupied by a destailed message, a list of custom layout. If you need to statae a simple message

2. Content area this can dispaly a message a list or other custom layout
3. action buttons there should be no more that three action buttons in a dialog

The alerDialog.Builder class provides APIs that allow you to creat an AlertDialog with these kinds of content including a custom layout. 
```
// 1. Instantiate an <code><a href="/reference/android/app/AlertDialog.Builder.html">AlertDialog.Builder</a></code> with its constructor
val builder: AlertDialog.Builder? = activity?.let {
    AlertDialog.Builder(it)
}

// 2. Chain together various setter methods to set the dialog characteristics
builder?.setMessage(R.string.dialog_message)
        .setTitle(R.string.dialog_title)

// 3. Get the <code><a href="/reference/android/app/AlertDialog.html">AlertDialog</a></code> from <code><a href="/reference/android/app/AlertDialog.Builder.html#create()">create()</a></code>
val dialog: AlertDialog? = builder?.create()
```

## add action buttons 
To add action buttons like those
```
val alertDialog: AlertDialog? = activity?.let {
    val builder = AlertDialog.Builder(it)
    builder.apply {
        setPositiveButton(R.string.ok,
                DialogInterface.OnClickListener { dialog, id ->
                    // User clicked OK button
                })
        setNegativeButton(R.string.cancel,
                DialogInterface.OnClickListener { dialog, id ->
                    // User cancelled the dialog
                })
    }
    // Set other dialog properties
    ...

    // Create the AlertDialog
    builder.create()
}
```
There are three different action butttons you can add

Positive
You should use this to accepthe and continue with teh action

Negative
You should use this to cancelt the action

Neutral
YOu should use this when the user may not want to proceed with the action, but doesn't necessarily want to cancel. It appears between the positive andd negative buttons. 

You can add only one of each button type to an alertdialog. that is you cannot have morethan one positiv ebutton. 

## Adding a list There are three kinds of lists available with te AlertDialog 
- A traditional single-choice list
- a Persistenet single-choice list(Radio buttons)
- A persistent mulitple-choice list(Checkboxes)

Single choice. 
```
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
```
Because the list appears in the dialog's content area, the dialog cannot show both a message and a list and you should set a tiltle for the dialog with setTitle(). To specify the items forth list, call setItems(), passsing an array. Alternatively, you can specify a list using setAdapter() this allows you to back the list with nynamic dta using a ListAdapter. 

If you choose to back your list with a ListAdapter, always use a loader so thta eh content loads asynchronously. THis is described further in Building layout and with an adapter dand the loaders guid. 


## adding persisten multiple-choice or single choice list
To add a list of multipel-choice items (checkboxes or single-choice items, use the setMultipChoiceItems() or setSingleChoiceItems() methods respectively. 
```
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
```

Although both a traditional ist and a list with radio buttons provide a single choice action, you should use setSingleChoiceItems() if you want to persist the users choice. That is if opening the dialog again later should indicate what the user's curretn choice is, then you createa  list with radio buttons. 

Creating a Custom layout
if you want toa custom layout in a dialog, create a layout and add it to an alertDialog by calling setView() on your AlertDialog.Builder object

By default the custom layout fills the dialog window but you can still use alertDailog.Builder methods to add buttons and a title. 

To inflate the layout in your dialogFragment, get a LayoutInflater with getLayoutInfalter() and call inflate(), where the first parameter is the layout resource ID and second parameter is a parent view for the layout. You can then call setView() to place the layout inthe dialog. 


## Passing events back tot he dialog's host
When the user touches one of the dialog's actions buttons or selects an item from  its list, your dialogFragmen tmight perform the necesssary action itself, but often you'll want to deliver the event to the activity or fragment that opened the dialog. TO do this, define an interface with a methods for each type of click event. Then implement that interface in teh host compoent that will receive the action events fromt he dialog



## Showing a Dialog
When you want to show your dialog, create an instance of your DialogFragment and call show() passsing the FragmentManager and a tag name for the dialog fragment. 

You can get the FragmentManager by calling getSupportFragemmntManager() from the fragmentActivity or getFragemtnManager() from a Fragment
```
fun confirmFireMissles() {
  val newFragment = fireMissilesdialogFragment()
  newFragment.show(supportFragmentManager, "missiles")
  
```
The second argument is a unique tag name that the system uses to save and restore the gragment state when necessary. The tag also allows you to get a handle to the fragment by calling findFragmetByTag(). 

## Showing a dialog Fullscreen or as aEmbedded fRagment
You might have a UI design in which you want to a piece of the UI to appear as a dialog insome situations, ut as a full screen or embeded fragment in others (pperhaps depending on whether the deviceis a large screen or small screeen). Th eDialogFragment class offers you this flexibility becuase it can still behave as an mbeddable fragment. 

However you cannot use AlertDialog.Builder or other Dialog object to buildl the dialog in this case. If you want to he dialogFragment to be embeddable, you must define the dialog's UI ina layout, then load the layout in the onCreateView() allbackk. 

Here's an examples DialogFragment that can appear as either a dialog or an embeddable fragment 

```
class CustomDialogFragment : DialogFragment() {

    /** The system calls this to get the DialogFragment's layout, regardless
    of whether it's being displayed as a dialog or an embedded fragment. */
    override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
    ): View {
        // Inflate the layout to use as dialog or embedded fragment
        return inflater.inflate(R.layout.purchase_items, container, false)
    }

    /** The system calls this only when creating the layout in a dialog. */
    override fun onCreateDialog(savedInstanceState: Bundle): Dialog {
        // The only reason you might override this method when using onCreateView() is
        // to modify any dialog characteristics. For example, the dialog includes a
        // title by default, but your custom layout might not need it. So here you can
        // remove the dialog title, but you must call the superclass to get the Dialog.
        val dialog = super.onCreateDialog(savedInstanceState)
        dialog.requestWindowFeature(Window.FEATURE_NO_TITLE)
        return dialog
    }
}

```


And here's some code that deciddes whether to show the fragment as a dialgo or a fullscreen UI based on the screen size. 

```
fun showDialog() {
    val fragmentManager = supportFragmentManager
    val newFragment = CustomDialogFragment()
    if (isLargeLayout) {
        // The device is using a large layout, so show the fragment as a dialog
        newFragment.show(fragmentManager, "dialog")
    } else {
        // The device is smaller, so show the fragment fullscreen
        val transaction = fragmentManager.beginTransaction()
        // For a little polish, specify a transition animation
        transaction.setTransition(FragmentTransaction.TRANSIT_FRAGMENT_OPEN)
        // To make it fullscreen, use the 'content' root view as the container
        // for the fragment, which is always the root view for the activity
        transaction
                .add(android.R.id.content, newFragment)
                .addToBackStack(null)
                .commit()
    }
}
```

The mIsLargeLayout boolean specifiese whether the current device should use the app's large layout design. The best way to set this kind of booolean is to declare a bool resource value withan alternative resource value for different screen size

res/values/bools.xml
```
<!-- Default boolean values -->
<resources>
    <bool name="large_layout">false</bool>
</resources>
```

res/values-large/bools.xml
```
<resources>
    <bool name="large_layout">false</bool>
</resources>
```

Then you can intialize the mIsLargeLayout value during the activity's oncreate
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    isLargeLayout = resources.getBoolean(R.bool.large_layout)
}
```


## Shwoing an activity as a dialog on large screens
Instead of shoing a dialog as a fullscreen UI when on small screens, you can accomplish the same result by showing an Activity as a dialog when on large screens. Which approach you choose depends on your app desing, but showing an activity as a dialog is often useful when your app is already designed for samlll screens and you'd like to imporve the experienceon tablets by shoing a shor-lived activity as a dialog. 

To show an activity as a dialog only when on  large screens, apply the 

## Dismissing a Dialog
When the user touches any of the action buttons created with an AlertDialog.builder the systme dismess the dialog for you

The system also dismisses the dialog when the user touches an item in a dialog list except when the list uses radio buttons or chckboxes. Otherwise, you can manually dismiss your dialog by calling dismiiss() on your DialogFragment

In case you need to perfrom certain actions when the dialgoo goes away, you can implement the onDismiss method your DialogFragment

You can also cancel a dialog. This is a special event that indicates the user explicitly left the dialog without completing the task. This occurs if the usser pressses the back button, touches the screen outsiide the dialog qarea, or if you explicitly call cancel() on the dialog

As shown in the examplle above, you can respond to the cancel event by implementing onCancel in your DialogFragment class. 
