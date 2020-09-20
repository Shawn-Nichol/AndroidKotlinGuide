# Adding a fragment to an activity
Usually, a fragment contributes a portion of the UI to the host activity, which is embedded as part of the activity's overall view hierarchy. There are two ways ou can add a fragment to the activity layout. 

Note the system require a unique identifier that the system can use to restore the fragment if the activity is restatred. There are two ways to provide an id
- android:id
- android:tag

## Declare in a layout
You can specify the layout provoerties for the fragment as if it were a view. The <fragment> element specifies the fragment to instantiate in the layout. When the system creates the activity layout, it instantiates each fragment specified in the layout and calls the onCreateView() method for each one, to retrieve each fragment's layout. The system inserts the View returned by the fragment directly in place of the fragment element. 

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.example.news.ArticleListFragment"
            android:id="@+id/list"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
    <fragment android:name="com.example.news.ArticleReaderFragment"
            android:id="@+id/viewer"
            android:layout_weight="2"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
</LinearLayout>
```

## Progragmically
At any time while your activity is running, you can add fragments to your activity layout. You simply need to specify a ViewGroup in which to place the fragment. 

To make fragment transaction in your activity (add, remove, replace), you must use APIs from FragmentTransaction.

```
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTransaction()
```

You can add a fragment using add(), the first argument is the container the fragment will go in, the second argument is the fragment. You must call commit to complete the transaction. 
```
val fragment = MyFragment()
fragmentTransaction.add(R.id.fragment_containter, fragment)
fragmentTransaction.commit()
```


## Managing Fragments
TO manage the fragment in your activity, you need to use FragmentManager. To get FragmentManager, call getSupportFragmentManager() from your activity. 
- GetFragments that exist in the activity, whith findFragmentById()  of findFragmentByTag().
- Pop Fragments off the back stack, with popBackStack().
- Register a listener for changes to the back stack, with addOnBackStackChagnedListener()


## Adding User Interface
A fragment is usually used as part of an activity's user interface contributes its own layout to the activity. To provide a lyoaut for a fragment, you must implement the onCreateView() callback methods, which the Android system calls when it's time for the fragment to draw its layout. The implementation of this method must return a View that is the root of your fragment's layout. 

To return a layout from onCreateView(), you can inflate it from a layout resource defined in XML. The container parameter passed to onCreateView() is teh parent ViewGroup(from the Activity layout) in which your fragment layout is inserted.

The inflate method takes three arguments
- The resource ID
- the ViewGroup
- A boolean indicating whether the inflated layout should be attache d tohte ViewGroup during inflating. In this case its false becasuse the system is already inserting the infaled alyout into the container passing true would create a redudndant ViewGroup in the final layout. 

```
class MyFragment : Fragment() {
  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    saveInstanceStateBundle?
  ): View {
    // Inflate the layout for this fragment
    return inflater.inflate(R.layout.my_layout, container, false)
  }
}
```

## Adding items to the App Bar
Your fragments can constribute menu items to the activity's OptionsMenu by implementing onCreateOptionsMenu(). In order for this method to recieve calls, however, you must call setHasOptionsMenu(). 

Any items that you then add to the options Menu from the fragment are appended to the existing menu items. The fragment also receives callbacks to onOptionsItemSelected() when a menu itme is selected. You can registera  view in your fragment layout to provide a context menu by calling registerForContextMenu(). When the user opens the menu, the fragment receives a call to onCreateContextMenu(). When the use rselectes an item, the fragment receives a call to onContextItemSelected(). 

Note: The activity is first to recieve the respective callback when the user selects a menu item. if the activity's  implementation of the on-item-selected callback does not handle the selected item, then the event is passed to the fragment's callback. This is true for the options menu and context menu. 


## Communicating with the activity
Although a fragment is an independent object it is directly tied to the activity that called it. 

Specifically, the fragment can access the FragmentActivity instance with getActivity() and easily perfrom tasks such as find view in the activity layout.
```
val listView: View? = activity?.findViewById(R.id.list)
```
Likewise, your activity can call methods in the fragment by acquiring a reference to the Fragment from FragmentManager, using findFragmentById() or findFragmentByTAg().
```
val fragment = supportFragmentManager.findFragmentByID(R.id.my_fragmnet) as MyFragment
```
