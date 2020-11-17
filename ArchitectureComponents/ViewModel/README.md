# ViewModel
The ViewModel class is used to store and manage UI-related data in a lifecycle concious way. THe ViewModel class allows data to survive confiuration changes. 

Architecture components provides ViewModel ehlper class for the UI controller that is responsible for prepaing data for the UI. ViewModel objects are automatically retained during configuratino changes so that data they hold is immediatelly avaliable to the next activity or fragment instance. 

If an activity is recreated it receives the same ViewModel instance that was created by the first activity. When the owner activity finishes the framework calls the ViewModel object onClear(). ViewModel objects are desgiend to outlive spcific instantiations of views or LifecycleOwners. This design also means you can write tests to cover a ViewModel more easily as it doesn't know about view and Lifecycle objects. ViewModel objects can contain LifecycleObservers, such as LiveData objects. However ViewModel objects must never observe changese to lifecycle-aware observalbes, such as LiveData objects. If the ViewModel needds the application context for example to find a system service, it can extend the Android ViewModel class and have constructor that receives teh Application in the constructor, since application classs extedns context. 

## LifeCycle
ViewModel objectst are scoped to the Lifecycle passed to the ViewModelProvider when getting the ViewModel. The ViewModel remains in memory until the Lifecycle it's scoped to goes away permanently in the case of an activity, when it fnishes, while in the case of a fragment when it's detached. 

You usually request a ViewModel the first time the system calls an activity object onCreate()method. The systme may call onCreate() several times throughout the life of an activity, such as when a device screen is rotated. The ViewModel exists from when you first request a ViewModel until the activity is finished and dsetoyed. 

## Replacing loaders with ViewModel
Loader classes llike CursorLoader are frequently used to keep the data in an app's UI in sync with a database. You can use ViewModel with few other classes, to replace the loader. Using ViewMOdel separates your UI controller from the data-loading operations, which means you have fewer strong references between classes. 

In one common approach to using loaders, an app might use a CursorLoader to observe the contents of a database. When a value in the database changes, the loader automatically triggers a reload of the data and updates teh UI. ViewMOdel works with Room and LiveData to replace the loader. The ViewModel ensures that the data survivies a device configuration change. Room informs your LiveData when the database changes, and the LiveData, in turn updates your UI with the revised data. 
