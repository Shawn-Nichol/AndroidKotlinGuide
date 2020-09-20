# Fragments
A fragment represents a behavior or a portion of the UI in a FragmentActivity. You can combine multiple fragments in a single activity to build a multi-pane UI and res-use a fragment in multiple activites. you can think of a fragment as a modular section of an activity, which has its own lifecycle, receives its own input events and which you can add or remove while the activity is running(like a sub activity).

A fragment must always be hosted in an activity and the fragment's lifecycle is directly affected by the host activity's lifecycle. For example, when the activity is paused, so are all fragments in it, and when the activity is destroyed, so are all fragments. However, while an activity is running, you can manipulate each fragment independently, such as add or remove them. When you perform a fragment transaction, you can also add it to a back stack that's managed by the activity each back stack entry in the activity is a record of the fragment transaction that occurred. The back stack allows the user to reverse a fragment transaction, by pressing the back button. 

When you add a fragment as a part of your activity layout, it lives in a ViewGroup inside the activity's view hierarchy and the fragment defines its own view layout. You can insert a fragment in to activity layot by declaring the fragment in the activity's layout file, as a <fragment> element or from your application code by adding it ot an existing ViewGroup. 

# Design philosphy
You should designe each fragment as a modular and reusable activity component. That is becuase each fragment defines its own layout and its own behavior with its own lifecycle callbacks, you can include fragments in multiple activities. This is important becuase a modular fragment allows you to change your fragment combination for differen screen sizes. When designing your application to support both tablets and handsets, you can reuse your fragments in different layout configurations to ooptimize the user experince based on teh available screen space. For example, on a handset, it might be necessary to separate framents to provide a single-pane UI when more than one fragment cannot fit. 


There are a few fragment subclasses

## DialogFragment 
Displays a floating dialog, using this class to create a dialog is a good alternative to using the dialog helper methods in the Activity class, becuase you can incorporate a fragment dialog into the back stack of fragments managed by the activity, allowing the user to reutrn to a dismissed fragment. 

## List Fragment
Display a list of items that are managed by an adapter similar to listActivity. It provides several methos for managing a list view, such as the onListItemClick() callback to ahandle click events. (Preffered method to display a list is recycler view).

## PreferenceFragmentCompat
Displays a hierarchy of preference objects as a list. This is used to create a setting screen for your application. 

