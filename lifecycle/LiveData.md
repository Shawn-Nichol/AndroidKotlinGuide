LiveData is adata hold class tha can be observed within a given lifecycle. This means that an Observer can be added in a pair with a Lifecycleowner, and this observer will be notified about modifications of the wrapped data only if the paired lifecycle Owner is in active state. LifecycleOwner is considered as active, if its state is Lifecycle.State.STARTED or Lifecycle.State.RESUMED. An observer added via observeForever(Observer) is considered as always active and thus will be always notified about modifications. For those observers, you should manually call removeObserver(Observer)

An observer added with a Lifecycle will be automatically removed if teh corresponding Lifecycle moves to Lifecycle.State.Destoryed. This is especially useful for activites and fragments where they can safely observe LiveData and not worry about leaks: they will be instanly unsubscribed when they are destoyed. 

In addition, LiveData has onActive() and onInactive() mehtods to get notified when number of active Observers change between 0 and 1. This allows LiveData to release any heavy resources when it does not have any Observers that are actively observing

This class is desgned to hold individual data fields of ViewModel, but can also be for sharing data between different modules in your application in a decoupled fashion
