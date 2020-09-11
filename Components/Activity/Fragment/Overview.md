# Fragments

A fragmetn represents a behavior or a portion of user interface in a FragemtnActivity. You can combine multiple framgents in a single activity to build a muliti-pane UI and reuse a fragment in multiple activites. You can think o f a fragment as a modular section of an activity, which has its own lifecycle, receives its own input events and which you can add or remove while the activity is running(sort of like a "sub activity" that you can reuse in different activites).

A gragment must alwways be hosted in an activity and the gragment's lifecycle is directly affected by the host activity's lifecycle. For example, when the activity is paused, so are all fragments in it, and when the activity is destroyed, so are all fragments. However, while an activity is running (it is in the resumed lifecycle state), you can manipulate each fragment independently, such as add or remove them. When you perform such as afragment trascation, you can also add it to a back stack that's managed by the activity each back stack entry in the activity is a record of the grament transaction that occurred. The back stack allows the user to reverse a fragment transaction (navigate backgwards), by pressing the back button. 

When you add a fragment as a part of your activity layout, it lives in a ViewGroup inside the activity's view hierarchy and the gragment defines its own view layout. You can insert a fragment inot activity layout by declaring the fragment in the activity's layout file, as a <fragment> element or from your application code by adding it to an existing ViewGroup


## Design philosophy
Android introduced fragments in Android 3.0, primiarly to support more dynamic and flexible UI designs on large screens, such as tablets. Becuase a tablet's screen is much larger than that of a handset, there's more room to combine and interchange UI components. Fragments allow such designs without the need for you to manage complex changes to the view hierarchy. By dividing the layout of an activity into fragments, you become able to modify the activity's apperaance at runtime and preserve those changes in a back stack that's managed by the activity. They are now widely available through the grament support library. 

For example, a news application can use one grament to show a list of articles on the left and another fragment to display an article on the right -both fragemtns appear in one activity, side by side, and each gragment has its own set of lifecycle callback methods and handle their own user input events. Thus instead of using one activity to select an article and another activity to read the article, thee user can select an article and read it all within the same activity. 

You should design each grament as a modular and reusable activity component. That is becuase each grament defines its own layout and it s own behavior with its own lifecycle callbacks, you can include on fragment in multiple activites, so you should design for reuse and avoid directly manipulating one fragment from another gragment. This is especially important becuase a modular gragment allows you to change your gragment combinations for different screen sizes. When designing your application to support both tablets and handsets, you can reuse your fragments in different layout configurations to ooptimize the user experince based on the available screen space. Fore example, on a handest, it might be necessary to separate fragments to provide a single-pane UI when more than one annot fit within the same activity. 

For example to continue with the news application example-the application can embed two gragments in Activity A when running on a tablet-sized device. However, on a handset-sized screen, ther's not enough room for both fragments, so Activity A includes only the gragment for the list of articles, and when the user selects an article, it starts Activity B,  which includes the second fragment to read the article. Thuse the application suppports both tablets and handsets by reusing gragments in different combinations 

## Create a fragment
TO create fragment you must create a subclass of Fragment (or anexisting subclass of it). THe fragment class has code that looks a lot like an activity. It contains callback  methods similar to an activity, such as onCreate(), onStart(), onPause(), ,and onStop(). In fact, if you 're converting an existing Android application to use fragments you might simply move code from your activity's callback methods into the respective callback methods of your fragment. 

Usually, you should implement at least the following lifecycle methods. 

### onCreate()
The system calls this when creating the fragment. Within your implementation, you should initialize essential componetns of the fragment that you want to retain when the gragemnt is paused or stopped, then resumed. 

### onCreateView()
The system calls this when it's time for the fragment to draw its user interface for the first time. To draw a UI for your fragment, you must return a View from this method that is the root of your fragment's layout. You can return null if the fragment does not provide a UI. 

### onPause()
This system calls this method  as the first indication that the user is leaving the fragment(though it doesn't alwasy meand the fragment is being destoryed). This is usually where you should commit any changes that should be persisted byond the current user session(Becuase the user might not come back)

Most applications should implement at least these three methods for every fragment, but there are several other callback methods you should also use th handle various stages of the fragment lifecycle. All the lifecycle callback methods are discussed in more detail in the section about Handling the Fragment LifeCycle. 

Note that the code implementing lifecycle actions of a dependent component should be placed in the component itself, rather than directly in the fragment callback implementations. See Handling Lifecycles with Lifecycle-Aware components to learn how to make your dependent componets lifecycle-aware. 

There are also a few subclass that you might want to extend, instead of the base fragemnt class

### DialogFragmetn 
Displays a floating dialog, Using this calss to create a dialog is a good alternative to using the dialog helper methods in teh Activity class, becuase oyuu can incorporate a gragemtn dialog into the back stack of fragmetns managed by the activity, allowing the user to return to a dismissed gragment. 

### ListFragment
Displays a list of items that are managed by an adapter (such as a simpleCursorADapter) simoilar to listActivity. It provides several methods for managing a list view, such as the onListItemClick() callback to handle click events. (Note that he preferred method for displaying a list is to use RecyclerView in it layout

### PreferenceFragmentCompat
Displays a hierarchy of Preference object as a list. This is used to create a setting screen for your application.

## Adding a user interface
A fragment is uaually used a s part of an activity's user intreface and contributes its own layout to the activity. 

To provide a layout for a fragment, you must implement the onCreateView() callback methods, which the  Android system calls when it's time for the fragment to draw its layout. Your implementation of this method must return a View that is the root of your fragment's layout. 

Note: If your fragment is a subclass of ListFragment, the default implementations retursn a ListView from onCreateView() so you don't need to implement it. 

To return a layout from onCreateView(), you can inflate it from a layout resource defined in XML. To help lyou do so, onCreateView(), you can inflate it from a layout resource defined in XML. To help you do so, onCreateView provides a LayoutINfalter object. 
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
The container parameter passed to onCreateView() is the parent ViewGroup(forom the Activity's layout) in which your fragment layout is inserted. The savedInstanceState parametere is a Bundle that provides data about the previous instance fo the fragment, if the fragment is being resumed(restoring state is discussed more in the section about Handling the Fragment Lifeccycle)

The inflate() method takes three arguments
- The resource ID of the layout
- The ViewGroup to be the parent  of the inflated layout. Passing the container is important in order for the sytem to apply layout parameters to the root view of the inflaled layout, specified by the parent view  in which it's going. 
- A boolean indicating whether the inflated layout should be attached to the ViewGroup (the second parameter) during inflation. In this case, this is false becuase the system is already inserting the inflated layout into the container passing true would create a redundant view group in the final layout.

## Adding a fragment to an Activity
Usually, a fragment continbutes a portion of the UI to the host activity, which is embedded as a part of the activit's overall view hierarchy. There are two ways you can add a fragment to the activity layout. 

- Declare the fragment inside the activity's layout file. 
In this case, you can specify layout properties for the fragment as if it were a vieew. For example, here's the layout file for an activity with two fragmetns:
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
The android:name attribute in the <fragment> specifies the Gragment class to instantiate in the layout. 
  
When the system creates this activity layout, it instantiates each fragment specified in the layout and calls the onCreateView() method for each one, to retrieve each fragment's layout. The system inserts the View returned bythe fragment directly in place of the <fragment> element. 
  
Note Each fragment requires a unique identifier that the system can use to restore the fragment if the activity is restarted ( and which you can use to capture the fragment ot perform transactions, such as remove it). there are two ways to provide an ID for a fragment
- android:id attribute with a unique ID.
- android:tage attribute witha unique string. 

### programically add the fragmento an existing ViewGroup
At any time while your actiivty is running, you can add fragments to your activity layout. You simply need to specify a ViewGroup in which to place the fragment. 

To make fragment  transactions in your activity (such as add, remoe or replace a fragment), you must use APIs from FragmentTransaction. You can get an innstance of FragmentTransaction from your FragmentAcitvity like this. 
```
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTransaction()
```

You can then add a fragment using the add() mehtdo, specifying the fragment to add and the view in which to insert it
```
val fragment = MyFragment()
fragmentTransaction.add(R.id.fragment_container, fragment)
fragmentTrasaction.commit()
```

The firs arguemtn passed to add() is the ViewGroup in which the fragment should be placed, specified by resource ID, and the second parametere is the fragment to add. Once you've made your changes with FragmentTrasactions, you must call commit() for the changes to take effect. 

## Managing Framgnets
To Manage the fragments in your activity, you need to use FragmentManager. To get it, call getSupportFragmentManager() from your activity. 

Some things that you can do with FragmentManager include
- Getfragments that exist in the activty, with findFragmentById()(for fragments that provide a UI in the activity layout) or findFragmentByTAg() for fragments that do or don't provide a UI.
- Pop fragments off the back stack, with popBackStack() (simulating a Back command by the user).
- Register a listener for changes to the back stack, with addOnBackStackChagnedListener().

For more information about these methods an others, refere to the FragmentManager class documentatoin. 

As demonstartated in teh previous section, you can also use FragmentManager to open a FragmentTransaction, which allows you to perfrom transactions, such as add and remove fragments. 

## Performing Fragment trasactions. 
A great feature about using fragments in your activity is the ability to add, remove, replacek and perfrom ohter actions with them, in response to user interaction. Each set of changes that you commit to the activity is called a transaction and you can perform one using APIs in FragmentTrasaction. You can also save each transaction to a back stack managed by the activity, allowing the user to navigate backward through the fragment changes (similar to navigating backward through activites. 

You can acquire an instance of FragmentTransaction from the FragmentManager
```
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTransaction()
```

Each transaction is a set of chnages that  you want ot perform at the same time. You can set up all the changes you want to perform for a given tranasaction using methods such as add(), remove(), and replace(). Then, to apply the transaction to the activity, you must call commit(). 

bEfore you call commit(), however, you might want to call addToBackStack(), in order to add the transaction to a back stack of fragment transactions. This back stack is managed by the actiivity and allows the user to return to the previous fragment state, by pressing the back button. 

For example, here's how you can replace on fragment iwth another, and perserve the previous state in the back stack.
```
val newFragment = MyFragment()
val transaction = supportFragmentManager.beginTransaction()
transaction.replace(R.id.fragment_container, newFragment)
transaction.addToBackStack(null)
transaction.commit()
```

In this example, newFragment  replaces whatever fragment is currently in the layout container identified by the R.id.fragment_containter ID. By calling addToBackStack(), the replace transaction is saved to the back stack so the user can reverse teh transaction and bring back the previous fragment by pressing the back button. 

FragmentActivity then automatically retrieve fragments from the back stack via onBackPressed()

If you add multiple changes to the transaction such as another add() or remove() and call addToBackStack(), then all changes applied before you call commit are added ot the back stack as a single transaction and the back button reverses them all together. 

The order in which you add changes to a FragmentTtransaction doesn't matter, expect:
- You must call commit() last.
- If you're adding multiple fragments to the same container, then the order in which you add them determines the  order they appear in the view hierarchy. 

If you don't call addToBackStack() when you perfrom a transaction that removes a fragment, then that fragment is destoyed when the transaction is committed and the user cannot navigate back to it. Whereas, if you do call addToBackStack() when removing a fragment, then the fragment is stopped and is later resumed i fthe user navigates back. 

Tip: For each fragment trasaction, you can apply a transition animation, by calling setTransition() before you commit. 

Calling commit() doesn't perform the transaction immediately. Rather, it schedules it to run on the activity's UI thread as soon as the thread is able to do so. If necessary , however you may call executePendingTransactions() from your UI thread to immediately execute transactions submitted by commit(). Doing so is usually not necessary unless the transaction is a dependency for jobs in other threads. 

## Communicating with the  Activity
Althougha  FRagment is implemented as an obejct that's independent from a FragmentActivity and can be used inside multiple activites, a given instance of a fragment is directly tied to the activity that hosts it. 

Specifically, the fragmen can access the FragmentActivity instance with getActivity() and easily perfrom tasks such as find a view in teh activity layout. 
```
val listView: View? = activity?.findViewById(R.id.list)
```
Likewise, your activity can call methods in the fragment by acquiring a reference to the Fragment from FragmentManager, using findFragmentById() or findFragmentByTAg().
```
val fragment = supportFragmentManager.findFragmentByID(R.id.my_fragmnet) as MyFragment
```
## Creating event callbacks to the activity
In some cases, you might need a fragment to share events or data with the activity and or the other fragments hosted by the activity. To share data, create a shared ViewModel, as outolined in the share data between  fragments section in the ViewModel guide. If you need to propagate events that cannot be handled with a ViewModel, you can instead define a clallback interface inside the fragment and require that the host activity implement it. When the activity receives a callback throughtt the interface, it can share the information with other fragments in the layout as necessary. 

For example, if a news application has two fragments in an activity one to show a list of articles (fragment A) and another to display an article (fragment B) then fragment A must tell the activity when a list item is selected so that it can tell fragment B to display the article. In this case, the OnArticleSelectedListener interface is declared inside fragment A

```
public class FragmentA : ListFragment() {
  ...
  container Activity must impleemnt this interface 
  intervace OnArticleSelectedListener {
    fun onArticleSelected(articleUri: Uri)
  }
  ...
}
```

Then the activity that hosts the fragment implements the OnArticleSelectedListener interface and overrides onArticleSeelcted() to notify fragment B of the event from fragment A. To ensure that the host activity implements this interface, fragment A's on onAttach() callback method (which the system calls when adding the gramgent to the activity) instantiates an instance of OnArticleSelectedListener by casting the Activity that is passed into onAttach(). 
```
public class FragmentA : ListFragment() {
  var listener: OnArticleSelectedListener? = null
  ...
  override fun onAttach(context: Context) {
    super.onAttach(context0
    listener = context as? OnArticleSelectedListener
    if(listener == null) {
      throw ClassCastException("$context must implement onArticleSeelctedListed")
    }
  }
  ...
}
```

If the activity hasn't implemented the interface, then the fragment throws a ClassCastEception . On success, the mListener member holds a reference to activity's implementation of OnARticleSelectedListener, so that fragment A can share events with teh activity by calling methods defined by teh onARticleSelectedListener interface. For example, if fragment A is an extension of ListFragment, each time the user clicks a list item, the system calls onListItemClick() in the fragment, which then calls onArticleSelected9) to share the event with the activity:
```
public class FragmentA : ListFragment() {
  var listener: OnARticleSelectedListener? = null
  ...
  override fun onListeItemClick(l: ListView, v: View, position: Int, id: Long) {
    // append the click item's ro ID with the content provider Uri
    val noteUri: Uri = ContentUris.withAppendedID(ArticleColumns.CONTENT_URI, id)
    // send the event and th Uri to the host activity
    listener?.onArticleSelected(noteUri)
  }
}
```

The id parameter passed onListItemClick() is the row ID of the clicked item, which the activity(or other fragment) uses to fetch the article from the applications ContentProvider.


## Adding items to the  App Bar
Your fragments can contribute menu items to the activit's OPtionsMenu (nad, consequently, the app bar) by implementing  onCreateOPtionsMen(). In order for this method to recieve calls, howoever, you must call setHasOPtionsMenu() during onCreate(), to indicate that the fragment would like to add items to the Options Menu. Otherwise, the fragment doesn't receive a call to onCreateOptionsMenu(). 

Any items that you then add to the Options Menu from the fragment are appended to the existing menu items. The fragment also receives callbacks to onOoptionsItemSelected() when a menu item is selected. 

you can also register a veiw in your fragment layout to provide a context menu by calling registerForcontextMen(). When the user opens the context menu, the fragment receives a call to onCreateContextMenu(). When the user selects an item, the fragment receives a call to onCtonextItemSelected(). 

Note: Although your fragment receives an on-item-slected callback for each menu item it adds, the activity is first to reeive the respective callback when the user selects a menu item. If the activity's implementation of the on-item-selected callback does not handle the selected item, then the event is passed to the fragment's callback. This is true for the Options Menu and context menus. 

## Handling the Fragment Lifecycle
Managing the lifecycle of a fragment is a lot like managing the lifecycle of an activity. Like an activity, a fragment can exist in thress states. 

Resumed: The fragment is visible in the running activity. 

Paused: Another activity is in the foreground and has focus, but the activity in which this gragment lives is still visible (the foreground activity is partially transparent or doesn't cover the entire screen)

Stopped: The fragmetn isn't visible. Either the host activity has been stopped or the fragment has been removed from the activity but added to the back stack. A stopped fragment is still alive (all state an dmember information is retained by the system). Hwoever, it is no longer visible to the user and is killed if the activity is killed

Also like an activity, you can presever the UI state of a fragment across configuration chagnes and process dath using combination of onSAveInstanceState(bundle), ViewModel, and persisten local stoarge. To learn more about preserving UI state, seee Saving UI States. 

The most significant difference in lifecycle between an activity and a fragment is how one is stored inits respective back stack. An activity is placed into a back stack of activites that's managed by the system when it's stopped, by default (so that the user can navigate back to it with the back button, as discussed in Tasks And Back STac). However a fragment is palced into a back stack amangered by the host activity only when you explicitly request that the instance be saved by calling addTo BackStack() during a transaction that removes the fragment. 

Otherwise, managing the fragment lifecycle is very similar to managing the activity lifecycle; the same practices apply. See th Activity Lifecycle guid and Handling Lifecycles with Lifecycle-aware Components to learn more about the activity lifecycle nd practices for maanging it. 

Caution: If you need a Context object within your Fragment, you can call getContext(). However, be careful to call getContext() only when the fragment is attached to an acitivty. When the fragment isn't attaached yet, or was detached ruing the end of its lifecycle, getContext() retuns null. 

## Coordinating with the activity lifecycle
The lifecycle of the acitvity in which the fragment lives directly affects the lifecycle of the fagment, such that each lifecycle callback for the activity results in a similar callback for each fragment. For example, when the activity receives onPause(), each fragment in the activity reeives onPause(). 

Fragments have a few extra lifecycle callbacks, howoever, that handle unique interaction with the activity in order to perform actions such as build and destory the fragment's UI. These additional callback methods are

### onAttach()
Called when the fragment has been associated with the activity

### onCreateView()
called to create the view hierarchy associated with the fragment. 

### onActivityCreated()
Called when the activity onCreate() method has returned. 

### onDEstoryView()
Called when the view hierarchy asssociated with the fragment is being removed. 

### onDetach()
called when the fragment is being disassociated from the activity. 

The flow of a fragment's lifecycle, as it is affect by its host activity, is illustartated by figure 3. In this figure, you can see how each successive state of the activit determines which callback methods a fragment may receive. For example, when the activiy has received it s onCreat() callback, a fragment in teh activity receives no more than the onActivityCreateed() callback.

Once the activity reaches the resumed state, you can feely add and remove fragments to the activity. Thus, only while the activity is in the resumed state can the lifecycle of a fragment change independently. 

However, whenthe activity leaves the resumed state, the fragment again is pushed through its lifecycle by the activity. 

