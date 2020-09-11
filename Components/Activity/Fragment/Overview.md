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
