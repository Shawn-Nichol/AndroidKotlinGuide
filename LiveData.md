LiveData is a data holder class that can be observed within a given lifecycle. This means a that an Observer can be added in a pair with a LifecycleOwner, and this observer will be notified about modiviations of the wrapped data only if the paired LifecycleOwner is ain active state. LifecycleOwner is considereed as active, if its state is Lifecycle.State.STARTED or Lifecycle.State.REsume. An observer added via observeForever is considered as always active and thus will be always notified about modifications. For those observers, you should manually call removeObserver

An observer added with a Lifecycle will be automatically removed if teh sorrespondingLifecycle moves to Lifecycle.State.DESTROYED state. This is especially useful for activites and fragments where they can safely observe LiveData and not worry about leaks: They willb instanly unsubscibed when they are destroyed. 

In additiona, LiveData has onActive and onInactive() methods to get notified when number of active Observers change between 0 and 1. This allows LiveData to release any heavy resources when it does not havee any Observers that are actiely observing

This class is designed to hold individual data fields of ViewModel, but can also be used for sharing data bnetween different modules in your application in yoa decoupled fashion. 

Observer is a callback tha can receive from LiveData. 
