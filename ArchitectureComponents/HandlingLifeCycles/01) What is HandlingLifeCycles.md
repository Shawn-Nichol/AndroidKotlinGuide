# Handling Lifecycle with LifecycleAware components
Lifecycle-aware components performa ctions in response to a change in the lifecycle status of another component, such as activites/fragments. These components help you produce better-oragnized code, and often lighter-weight code that is easier to maintain. 

A common pattern it to implement the actions of the dependen components in the lifecycle methods of activites and fragments. However this pattern leads to a poor organization of code and to the proliferation of errors. By using lifecycle-aware components, you can move the code of dependent cmoponent out of the lifecycle methods and into the components themselves. 

Lifecycle  is a class that holds the inforrmation about the lifecycle of a component like an activity/fragemnt and allows other objects to observe this state.

## Event 
The lifecycle events are dispatched from the framework and the lifecycle class. These events map to the callback event in activities/fragments.

## State
The current state of the conponent tracked by the Lifecycle object. Think of the state as nodess of a graph and events as the edges between these nodes. 

## LifecycleOwner
LifecycleOwner is a single method interface that denotes that the class has a lifecycle. It has one method, getLifecycle(), which must be implemented by the class. This interface abstracts the ownership of a lifecycle from individual classes, such as Fragment and AppCompatActivity, and allows wrigting componetns that work with them. Any custom application class can implement the LifecycleOwner interface. 

Components that implement LifecycleObserver work seamlessly with components that implement LifecycleOwner because an owner can provide a lifecycle, which an observer can register to watch. 

## Use Cases for Lifecycle-aware components
Lifecycle-aware componets can make it much easier for you to maange lifecycles in a varitety of cases. 

- Switching between coarse and fine-grained location updates. Use lifecycle-awre components to enable fine-grained location updates while your locatio app is visible ans switch to coarse-grained updates when the app is in the backgroun. LiveData, a lifecycle-aware componet allows your app to automaticallly updat ehte UI when your user dhanges locations. 

- Stoppping and starting video buffering. Use lifecycle-aware components to start video bufferring as soon as possible, but defer playback until app is fully started. You can also use lifecycle-aware componetns to elminate buffering when your app is destoryed

- Starting and stopping network connectivity. Use lifecycle-aware componetns to enable live updating of network data while an app is in the foreground and also to automaticallly pause when the app goes into the background. 

- Pausing and resuming animated drawables. Use Lifecycle-aware componetns to handle pausing animated drawables when while app is while the app is in the background and resume drawables after the app is in the foreground. 

