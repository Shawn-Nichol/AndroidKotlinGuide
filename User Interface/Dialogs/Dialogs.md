




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




