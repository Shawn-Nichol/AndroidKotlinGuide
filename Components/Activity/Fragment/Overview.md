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
