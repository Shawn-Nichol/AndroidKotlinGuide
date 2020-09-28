# Handling Lifecycles with LifecycleAware componetns
Lifecycle-aware componets perform actions in response to a change in the lifecycle status of another component, such as activites and fragments. These components help you produce better-organized, and often lighter-wieght  code. that is easier to maintin. 

A common patter is to implement the actions of the dependent componets in the lifecycle  methods of activites and fragments. However, this pattern leads to a poor organization of the code and to the proliferation of errors. By using lifecycle-aware componetns, you can move the code of depenedent componetns out o fhte lifecycle methods and into the components themselves. 

The androidx.lifecycle packagae provides classes and interfaces that let you build lifecycle-aware components which are componnets that can automatically adjust their behavior based on the current lifecycle state of an activity or fargment. 

Note: to import androidx.lifecycle into your android progject, see the instructions for declaring dependencies in the Lifecycle release notes. 

Most of the app components that are defined in the Andorid fremwork have llifecycles attached to them. Lifecycles are managed by the operating system or the framework code runningin your process. They are core to how android works and your application must respect them. Not doing so may trigger memory leaks or even application crashes. 

Imagine we have an activity that shows the device locationon the screen. A common implementation might be like the following.


Even thought this sample looks fine, in a real app, you end up having too many class that manage the UI and other componetns in response to the current state of the lifecycle. Managing multiple components places considerable amount of code in lifecycle methods, such as onStart() and onStop(), which makes them difficult to maintin. 

Moreover, there's no guarantee that the component starts before the activity or fragment is stopped. This is especially true if w eneed to perform a long-running operation, such as some configuration check in onStart(). This can cause a race condition where the onStop() method finishes before othe onStart(), keeping the the componetn alive longer than it's needed. 

The androidx.Lifecycle package provides classes and interfaces that help you tackle these problems in a resilient and isolated way. 

Lifecycle
Lifecycle is a class that hold the information about the lifcycle state of a component like an activity or fragment and allows other object to observe this state. 

Lifecycle uses two main enumerations to track the lifecycle status for ist associated component. 

### Event
The lifecycle events that are dispatched fromt he framework and the lifecycle class. These events map to the callback events in activites and fragments. 

### State
the current state of teconponent tracked by the Lifecycle object. 

Think of the states as nodes of a graph and events as the edges between these nodes. 

A class can monitor the component's lifecycle status by adding annotations to its methods. Then you can add an observer by calling the addObserver() method of the Lifecycle class and passing an instance of your observer. 
```
class MyObserver : LifecycleObserver {
  @OnLifecycleEvent(Lifecyle.Event.ON_RESUME)
  fun connectListener() {
    ...
  }
  
  @OnLifecycleEvent(Lifecycle.EVENT.ON_PAUSE)
  fun disconnecteListener() {
    ...
  }
}

myLifecycleOwner.getLifecycle().addObserver(MyObserver())
```

The myLifecycleOwner object implements the LifecycleoWner interface.


## LifeCylceOwner
LifecycleOwner is a s sinlge method interface that denotes that the class has a Lifecycle. It has one method, getLifecycle(), which must be implemented by the class. If you're trying to manage the lifecycle of a whole application process instead.

This interface abastracts the ownership of a lifecycle from individual classes, such as Fragment and AppCompatActivity, and allows writing components that work with them. Any custom application class can implement the LifecycleOwner interface. 

Compoonetns that implement LifecycleObserver work seamlessly with components that implement LifecycleOwner becuase an owner cna provide a lifecycle, which an observer cana register to watch. 

For the location tracking example, we can make the  MyLocationListener class implement LifecycleObserver and then initialize it with the activity's Lifecycle in the onCreate() method. This allows the MyLocationListener class to be self-sufficient, meaning that logic to mreact to changes in lifecycle status is declared in MyLocationListener instead of the activity. Having the individual components sotre their own logic makes the activites and fragments logic easier to manage. 
```
class MyActivity : AppCompatActivity() {
    private lateinit var myLocationListener: MyLocationListener

    override fun onCreate(...) {
        myLocationListener = MyLocationListener(this, lifecycle) { location ->
            // update UI
        }
        Util.checkUserStatus { result ->
            if (result) {
                myLocationListener.enable()
            }
        }
    }
}
```
A commmon use case is to avoid invoking certain callbacks inf the lifecycle isn't in a good state right now. For example, if the callbck runs a fragment transaction after the activity state is saved, it would trigger a crash, so we would never want to invoke that callback. 

To make this use case easy, the Lifecycle class allows other  objects to query the current state. 

```
internal class MyLocationListener (
  private val context: Context,
  private val lifecylce: Lifecyle,
  private val callback: (Location -> Unit
) {

private var enabled = false

@OnLifecycleEvent(Lifecycle.Event.ON_START)
fun start() {
  enabled = true
  if(lifecycle.currentState.isAtLeast(Lifecycle.State.STARTED) {
    // connect if not connected
  }
}

@OnLifecycleEvent(Lifecycle.Event.ON_STOP) 
fun stop() {
  // disconnect if connected. 
}
```

With this implementation, our LocationListener class is completely Llifecyle-aware. If we need to use our LocationListener from another activity or fragment, we just need to initialize it. All of the setup and teardown operaations are managed by the class itself. 

If a library provides classes that need to work with the Android lifecycle, we recommend that you use lifecycle-aaware components. Your Library clients can easily intergrate those components without manual lifecycle managemnt on the client side. 

## Implementing a custom LifecycleOwner
Fragments and Activites in Support Library 26.1.0 and later already implement LifecycleOwner interface.

If you have a custom class that you would like to make a LifecycleOwner, you can use the LifecycleRegistry class, but you need to foward events into that class. 

```
class MyActivity : Activity(), LifecycleOwner {
  private lateinit var lifecycleRegistry: LifecycleRegistry
  
  overrid fun onCreate(savedInstanceStae: Bundle?) {
    super.onCreate(savedInstanceState: BUndle?)
    lifecycleRegistry = LifecyclRegistry(this)
    lifecycleRegistry.markState(Lifecycle.State.CREATED)
  }
  
  public override fun onStart() {
    super.onStart()
    lifecycleRegistery.markState(Lifecycle.State.STARTED)
  }
  
  override fun getLifecycle(): Lifecycle {
    return lifecycleRegistry
  }
}

```

## Best practices for lifecycle-aware components
- Keep your UI controllers as lean as possible. They should not try to acquire their own data; instead, use a ViewModel todo that and observe a LiveData object to refelct the changes back to the views. 

- Try to write data-driven UIs where your UI controllers responsibility is to update the views as data changes, or notify user actions back to the ViewModel.

- Put your data logic in your ViowModel class. ViewModel should serve as the connector between your UI controller and the rest of your app. Be careful through, it isn't ViewModel's responsibility to fetch data. Instead, ViewModel should cal the appropriate component to fetch the data, then provide the result back to the UI controller. 

- Use DataBinding to maintain a clean interface between your views and the UI controller. THis allows you to make your views more delcarative and minimize the update code you need to write in your activites and fragments. If you prefer to do this in the java progamming language, use a library like Butter knife to avoid boilerplate code and have a better abastraction

- If your UI is complex, consider creatinga presenter class to handle AUI modifications. This might be a laborious task, but it can make your UI componets easier to test. 

- Avoid referencing a view or Activity context in your ViewModel. If the ViewModel outlives the activty, your activity leaks an disn't properly dispoed by the garbage collector. 

- Use kotlin coroutines to manage long-running tasks and other operartions that can run asyncrhounsly. 

## Use cases for lifecycle-aware components
Lifecycle-aware componets can make it much easier for you to manage lifecycles in a varitety of cases.

- Switching between coarse and fine-grained location updates. Use lifecycle-aware components to enable fine-grained location updates while your location app is visible and switch to coarse-grained updates when the app is in the background. LiveData, a lifecycle-aware componet allows your appp to automatically update the UI when your user changes locations. 

- Stopping and sstaring video buffering. Use lifeycle-aware components to start video buffering as soon as possible, but defer playback until app is fully started. You can also use lifecycle-aware components to erminate buffering when your app is destoryed. 

- Starting and stopping network connectivity. Use lifecyle-aware components to enable live updating of network data while an app is in the foegoround and also to automatically pause when the app goes into the backgroun. 

- Pausing and resuming animated drawables. Use Llifecycle-aware component to handle pausing animated drawables when while app is in the bckgroudn and resume drawables after the app is in the foreground.

## Hanldin gon stop events
Whe a lifecylce belongs to an AppCOmpatActivity or Fragment, the Lifecycle's state chagnes to created and the ON_STOP event is dispatech when the AppCompatAcitivty or Fragment's onSAveInstanceState is called. 

When a fragment or AppCompatActivity's state is saved via onSaveInstanceState(), it's UI is considered immutable until ON_START is called. Trying to modify the UI after the state is saved is likely to cuase inconsistencies in the navigation state of your application which is why FragmentManager throws an exception if the app runs a FragmentTrasaction after state is saved. See commit() for details. 

LiveData prevents this edge case out of the box by refraining from calling its observer if the observer's associated Lifecycle isn't at least STARTED. Behind the scens it calls  isAtleast() before deciding to invoke it observer. 

Unfortunately, AppCopmatAcitivty's onStop() method is called after onsaveInstanceState(), which leaves a gap where uI state changes are not allowed but the Lifecycle has not yet been moved to the CREATED state. 

To prevent this issue, the Lifecycle class in version beta2 and lower mark the state as CREATED without dispatching the event so that any code that checks the currrent state gets the real value even though the event isn't dispatech utni lonStop() is called by the system. 

Unfortunately this solution has two major problems


