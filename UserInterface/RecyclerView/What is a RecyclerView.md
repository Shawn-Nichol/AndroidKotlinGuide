# What is RecyclerView
The RecyclerView widget  is a more advanced and flexible version of ListView. In RecyclerView model, several different componetns work together to display your data. The overall container for your user interface is recyclerView object that you add to your layout. The RecyclerVeiw fills itself with views provided by a layoutmanager that you provide. you can use on eof our standard layout managers (such as LinearlayoutManager or GrigLayoutManager or implement your own. 

The views in the list are respresented by the view holder objects. Theses objects are instance of a class you define by extending RecyclerView.ViewHolder. each View holder is in charge of displayiing a single item with a view. For example, if your list shows music collection, each view holder might represent a single album. The RecyclerView creates only as many view holders as are needed to display the on-screen portion of the dynamic content, plus a few extra. As the user scrolls through the list, the RecyclerView takes the off-screen views and reginds them to the data which is scrolling on to the screen

The view holder objects are managed by an adapter, which you create by extending RecyclerView.Adapter. The adpater creates view holders as needed. The adapter also binds the view holders to their data. It does this by assinging the view holder to a position, and calling the adaptere's onBindViewHolder() methdos That method uses the viw holder's position to determine what the contents should be, based on  its list position. 

This RecyclerView model does a lot op optimization work so you don't have to

- When the list is first populated, it creteas and binds some view holders on either side of the list. For example, if the view is displayign list positions 0 through 9, the RecyclerView creates and binds those views holders, and might also create and bind the view holder for position 10. That way, if the user scrolls the list, the next elemetn is ready to display. 

- As the user scrolls the list, the RecyclerView creates new view holders as necessary. It also saves the view holders which have scrolled off-screen, so they can be reused. If the ser switches the direction they were scrolling, the view holders which were scrolled off the screen can be brought right back. On the other hand, if the user keeps scrolling in the same direction, the view holders which have been offf-screen the longest can be re-bound to new data. The view holder does not need to be createed or have its view inflated; instead the app just updates the view's contents to match the new item it was bound to. 

- When the displayed items chagne, you can notify the adapter by calling an appropriate REcyclerView.Adapter.notify...(). The adapters built-in code then rebinds just the affected items. 

## Add support library
dependencies {
    implementation 'com.android.support:recyclerview-v7:28.0.0'
}

## add ReyclerView to your layout
```
<?xml version="1.0" encoding="utf-8"?>
<!-- A RecyclerView with some commonly used attributes -->
<android.support.v7.widget.RecyclerView
    android:id="@+id/my_recycler_view"
    android:scrollbars="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

Obtain a handle to the object, connect it to a layout manager, and attach an adapter for the data to be dsiplayed. 
```
class MainActivity : Activity() {
      private lateinit var recyclerView: RecyclerView
    private lateinit var viewAdapter: RecyclerView.Adapter<*>
    private lateinit var viewManager: RecyclerView.LayoutManager
    
        override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.my_activity)

        viewManager = LinearLayoutManager(this)
        viewAdapter = MyAdapter(myDataset)

        recyclerView = findViewById<RecyclerView>(R.id.my_recycler_view).apply {
            // use this setting to improve performance if you know that changes
            // in content do not change the layout size of the RecyclerView
            setHasFixedSize(true)

            // use a linear layout manager
            layoutManager = viewManager

            // specify an viewAdapter (see also next example)
            adapter = viewAdapter

        }
    }
} 
```

## Add a list adapter
To feed all your data to the list, you must extend the RecyclerViewAdapter class. THis object creates views for items, and replaces the content of some of the ivew with new data items when the orignal item is no longer visible. 

The following code example shows a simle implementation for a a data set that consists of any array of string displayed using TextView widgets
```
class MyAdapter(private val myDataset: Array<String>) :
        RecyclerView.Adapter<MyAdapter.MyViewHolder>() {

    // Provide a reference to the views for each data item
    // Complex data items may need more than one view per item, and
    // you provide access to all the views for a data item in a view holder.
    // Each data item is just a string in this case that is shown in a TextView.
    class MyViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView)


    // Create new views (invoked by the layout manager)
    override fun onCreateViewHolder(parent: ViewGroup,
                                    viewType: Int): MyAdapter.MyViewHolder {
        // create a new view
        val textView = LayoutInflater.from(parent.context)
                .inflate(R.layout.my_text_view, parent, false) as TextView
        // set the view's size, margins, paddings and layout parameters
        ...
        return MyViewHolder(textView)
    }

    // Replace the contents of a view (invoked by the layout manager)
    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        // - get element from your dataset at this position
        // - replace the contents of the view with that element
        holder.textView.text = myDataset[position]
    }

    // Return the size of your dataset (invoked by the layout manager)
    override fun getItemCount() = myDataset.size
}
```

The Layout manager calls teh adapter's onCreateViewHolder() method. That mehtod needs to construct a RecyclerView.ViewHOlder and set the view it uses to display its contents. The type of the ViewHolder must match the type declared in the adpater class signature. Typically, it would set the veiw by inflating an XML layout file. Becuase the view holder is not yet assigned to any particcular data, the method does not actually set the view's contents. 

The layout manager then binds the view holder to its data. It does this by calling the adapter's onBindViewhOlder() method, and passing the view holder's position in the ReycclerView. The onBindViewHOler90 method needsd to fetch the appropriate data, and us it to fill in the view holder's layout. For example, if the ReyclclerView is displaying a list of names, the method might find the appropriate name in the list, and fill in the view holder textVeiew widget. 

If the list needs an update, call a notification method on the RecyclerView.adapter object, such as notiftyItemChanged(). The layout manger then reginds any affected view holders, allowing theier data to be updated. 

## Customize RecyclerView

Modify the layout
The RcyclerView uses a layout manager to positio the individual items on the screen and determine when to ureuse items views that are no longer visible to the user. The reuse a view, a layout manager may ask the adapter to replace the contents of the view with a different element from the dataset. Recycling views in this manner imporves performance by avoiding the creation of unnecessary views or performing expensive findViewById() lookups. The Android Support library includes three standard layout managers, each of which offers many customization options. 

- LinearLayoutManger: arranges the items in a one-dimensional list. using RecyclerView with LinearLayoutmanager provides functionality like the older ListView layout.

- GridLayoutManager: arranges the items in a two-dimensional grid, like the squares on a checkerboard. Using a RecylerVIew with GridLayoutManager provides functionality like th eolder GridView layout.

- StaggeredGridLayoutManager: arranges the items in a two-dimensional grid, with each column slightly offset from the one before, like the stars in ana American flag. 

If none of these layout managers suit your needs, you can create your own by extending the RecyclerView.LayoutManager abstract class. 

## Add Item animations
When an item changes, the RecyclerView uses an animator to change its appearcance this animator is an objec taht extends the abstract RecyclerView.ItemAnimator class. By default, the RecyclerView usese DefaultItemAnimator to provide the animation. If you want to provide custom animations, you can define your own animator object by extending RecyclerView.ITemAnimator. 

## Enable list-item selection

The REcyclerView-selection library enables users to select items in RecyclerView list using touch or mouse input. You retain control over the visual presentation of a selected item . You can also retain controlo ver policeeis controlling selection behavior such as items that can be eligible for selection, and how many items can be selected. 

To add Selection support ot a RecyclerView instance, follow these steps. 
1) Determin which selection key type to use, then build a ItemKeyProvider. 

There are three key types that you can use to identify selected items: Parcelable and all sublcasses like Uri), String and long For detailed information about selection-key types, see SelectionTracker.Builder

2) Implement ItemDetailsLookup
ItemDetailsLookup enables the selection library to access information  about RecyclerView items given a MotionEvent. It is effecitvely a factory for Item Details instances that are backed up by a RecyclerView.ViewHolder instanc.e 

3) Update item Views in RecyclerView to reflect that the user has selecged or unselected it. 
The selectio library does not provide a defualt visual decoratio for the  selected items. you must provide this when you implement onBindViewHolder(). The recommended approach is

- In onBindViewHolder(), cal lsetActivated() (not setSelected()) on the view object with true or false

- Update the styling of the view to represent the activated status. We recommend you use a color state list resource to configure the styling. 

4) Use ActionMode to provide the user with tools to perform an action on the selction.

Register a selectionTracker.SelectionObserver to be notified when selection changes. When a selection is fisrt created, start Action Mode to represent this to the user, and privde selection-specific actions. For example, you may add a deletee  button to the ActionMode bar, and connect the back arrow on the bar to clear the selection.  When the selection becomes empty (if the user cleared the selection the last time), don't forget to terminate action mode. 

5) Perform an interpreted secondary actions 

At th the end of the event processing pipeline, the library may determine that the user is attempting to activate an item by tapping it, or is attempting to drag and drop an item or set of selected items. React to these interpretations by registering the appropriate listener. For more information see selectionTracker.builder. 

6) Assembe everything with SelectionTracker.Builder
```
var tracker = SelectionTracker.Builder(
    "my-selection-id",
    recyclerView,
    StableIdKeyProvider(recyclerView),
    MyDetailsLookup(recyclerView),
    StorageStrategy.createLongStorage())
        .withOnItemActivatedListener(myItemActivatedListener)
        .build()
```

In order to build a selectionTracker instance, you app must suplly the same RecyclerView.Adapter that you used to initialize RecyclerView to SelectionTracker.Builder. For this reason, you will most likely need to inject the SelectionTracker instance, once created, into your RecyclerView.Adapter after the RecyclerView.Adapter is created. Otherwise, you won't be able to check an item's selected status from the onBindViewHolder().

7) Include seelction in the activity lifecycle events

In ordre to preserve selection stat across the activity lifecycle events, your app must cal the selection trackers onSaveInstanceState90 and onRestoreInstanceState() methods from the activity's onSaveInstanceState() and onREstoreInstanceState() methods respectively. Your app must also supply a unique selection ID to the SelectionTracker.Builder constructor. This ID is required becuase an activity or a fragment may have more than one distinct, selectable list all of which need to be persisted in their saved state. 
