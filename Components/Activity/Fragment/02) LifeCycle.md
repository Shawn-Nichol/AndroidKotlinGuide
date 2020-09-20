# Create a Fragment
The flow of a fragment lifecycle, is affected by its host activity. Once the activity enters the resumed 


Like an activity, you can preserver the UI state of a fragment across configuration changes and process death using a combination of onSaveInstanceState(bundle) and ViewModel.

The most signicant difference between activity and fragment lifecycles is how on eis stored in respective to the back stack. An activity is placed into the back stack of activities that's managed by the system when it's stopped, by default. However a fragment is placed into a back stack manager by the host activity only when explicitly request that the instance be saved by calling addToBackStack() during a fragment transaction. 

If you need a context object within your fragment, you can call getContext(). However, be careful to call getContext() only when the fragment is attached to an activity. When the fragment isn't attached yet, or was detached during the end of its lifecycle, getContext() returns null. 

## onAttac()
Called when the fragment has been associated with the activity. 

## onCreate()
The system calls this when creating the fragment. Within your implementation, you should initialize essential components of the fragment that you want to retain when the fragment is paused or stopped, then resumed. 

## onCreate()
The systme calls this when it's time for the fragment to draw its user interface for the first time. To draw a UI for your fragment, you must return a View from this method that is the root of your fragment's layout. you can return null if the fragment does not provides a UI. 

## onActivityCreated()
Called when the activity onCreate() method has returned. 

## onPause()
Another activity is in the foreground has focus, but the activity in which this fragment lives is stilll visible. 

## resumed()
The fragment is visible in the running activity

## stopped()
The fragment isn't visible. Either the host activity has been stopped or the fragment has been removed from the activity but added to the back stack. A stopped fragment is still alive. However it is no longer visible to the user and is killed if the activity is killed. 

## onDestroyView()
Called when the view hierarchy associated with the fragment is being removed. 

## onDetach() 
Called when the fragment is being disassociated from the activity

