# LiveData
LiveData is a data holder class that can be observed within a given lifecycle. This means that an Observer can be added in a pair with a Lifecycleowner, and this Observer will be notified about modifiactions of the wrapped data only if the paired lifecycle owner is in an active state. LifeCycleOwner is consider active, if it in the Lifecycle.State.STARTED or LifeCycle.State.RESUMED. An observer is added via observeForever(Observer is considered as alwasy active and thus will always be notified about modifications. For those observers, you should manually call removeObserver(Observer)

An observer added with a LifeCycle will be automatically removed if the corresponding Lifecycle moves to Lifecycle.State.Destroyed. This is especially useful for activites and fragments where they can safely observe LiveData and not worry about leaks: They will be instanly unsubscribed when they are destoryed. 

In addition, liveData has onActive() and onInactive() methods to get notified when the number of active Observers change between 0 and 1. This allows LiveData to release any heavy resources when it does not have any Observers that are actively observing. This class is designed to hold individual data fields of ViewModel, but can also be for sharing data between different modules in your application in a decoupled fasion. 

# LiveData Override
Livedata is an observable data holder class. Unlike a regular observable, LiveData is lifecycle-aware, respecting the lifecycle of other app components, such as activites, fragments, or services. This awarenesss ensures LiveData only updates app components that are in a active lifecycle state. 

LiveData is considered an observer, which is represented by the observer class, to be in an active state if its lifecycle is in the STARTED or RESUMED state. LiveData only notifies active observers about updates. Inactive observer registered to watch LiveData object aren't notified about changes. 

You can register an observer paired with an object that implements the LifecycleOwner interface. This relationship allows the observer to be removed when the state of the corresponding Lifecycle object changes to DESTORYED. This is especially useful for activites and fragment becuase they can safely observe LiveData objects and not worry about leaks, activites and fragments are instanly unsubscribed when their lifecycles are destroyed. 

The advantages of using LiveData
### No Memory leaks
Observers are bound to lifecycle objects and clean up after themselves when their associated lifecycle is destroyed. 

### No crashes due to stopped activites
If the observer's lifecycle is inactive, such as in the case of an activity in the back stack, then it doesn't receive any LiveData events

### No more manual lifecycle handling 
UI components just observe relevant data and don't stop or resume observations. LiveData automatically manages all of this since it's aware of the relevant lifecycle status changes while observing. 

### Always up to date data
If a lifecycle becomes inactive, it receives the latest data upon becoming active again. For example, an activity that was in teh background receives the latest data  right after it returns to the foreground. 

### Proper configuration changes
If an activity or fragment is recreated due to a configuration change, like device rotation, it immediately receives the latest available data.  

### Sharing resources
You can extend a LiveData object using the singleton pattern to wrap system services so that they can be shared in your app. The LiveData object connects to the system service once, and then any observer that needs the resource can just watch the LiveData object. 

