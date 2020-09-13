LiveData is adata hold class tha can be observed within a given lifecycle. This means that an Observer can be added in a pair with a Lifecycleowner, and this observer will be notified about modifications of the wrapped data only if the paired lifecycle Owner is in active state. LifecycleOwner is considered as active, if its state is Lifecycle.State.STARTED or Lifecycle.State.RESUMED. An observer added via observeForever(Observer) is considered as always active and thus will be always notified about modifications. For those observers, you should manually call removeObserver(Observer)

An observer added with a Lifecycle will be automatically removed if teh corresponding Lifecycle moves to Lifecycle.State.Destoryed. This is especially useful for activites and fragments where they can safely observe LiveData and not worry about leaks: they will be instanly unsubscribed when they are destoyed. 

In addition, LiveData has onActive() and onInactive() mehtods to get notified when number of active Observers change between 0 and 1. This allows LiveData to release any heavy resources when it does not have any Observers that are actively observing

This class is desgned to hold individual data fields of ViewModel, but can also be for sharing data between different modules in your application in a decoupled fashion


# LiveData Overrive
LiveData is an observable data hold class. Unlike a regular observable, LiveDAta is lifecycle-aware, maning it respects the lifecycle of other app components, such as activities, fragments, or services. This awareness ensures Livedata only updates app components observers that are in an active lifecycle state. 

LiveData is considered an obserer, which is represented by the Observer class, to be in an active state if its lifecycle is in the STARTED or RESUMED state. LiveData only notifies active observers about updates. Inactive observer registered to watch LiveData object aren't notified about changes. 

You can register an observer paied iwth an object that impelemtns the Lifecycleowner interface. This relationship allows the observer to be removed when the state of teh corrrespoonding Lifecycle object changes to DESTROYED. This is especially useful for activites and fragemnts becuase they can safely observe LiveData objects and not worry about leaks activities and fragements are instantly unsubscribed when their lifecycles are destroyed. 

The advantages of using LiveData
Ensures your UI matches your data state
LiveData follows the observer pattern. LiveData notifies Observer objects when the lifecycle state changes. You can consolidate your code to  update the UI in these Observer objects insteaed of updating the UI every time the app data changes, your observer can update the UI every time there's a change. 

### No Memory leaks
Observers are bound to Lifecycle objects and clean up after themselves when their associated lifecycle is destoryed. 

### No crashes due to stopped activites
If the observer's lifecycle is inactive, such as in the case of an activity in the back stack, then it doesn't receive any LiveData events

### No more manual lifecycle handling 
UI components just observe relevant data nd don't stop or resume observations. LiveData automatically manages all of this since it's aware of the relevant lifecycle status changes whil observing

### Always up to date data
If a lifecycle becomes inactive, it receives the latest data upon becoming active again. For example, an activity that was in the background receives the latest data right after it returns to the foreground

### Proper configuration changes
If an activity or fragment is recreated due to a configuration change, like device rotation, it immediately receives the latest available data. 

### Sharing resources
You can extend a LiveData object using the siingleton pattern to wrap system services so that they can be shared in your app. The LiveData object connects to the system service once, and then any observer that needs the resource can just watch the LiveData object. For more information see Extend LiveData. 

## Work With LiveData objects
1) Create an instnce of LiveData to hold a certain type of data. This is usually done within your ViewModel class

2) Create an Observer object that defines the onCanged(), which controls what happens when the LiveData object's held data changes. You usually create an Observer object in a UI controller, such as an activity or fragment. 

3) Attach teh Observer object to the LiveData object using the observe() method. The observe() method taks a Lifecycleowner object. This subscribes the Observer object to the LiveData object so that it is notified of changes. You usually attach the Observer object in a UI controller, such as an activity or fragment. 

When you update the value stored in teh LiveData object, it triggers all registered observers as long as the attached LifecycleOwner is in teh active state. 

LiveData allows UI controller observer  to subscribe to updates. When the data held by the LiveData object changes, the UI automatically updates in response. 

## Create LiveData Object
LiveData is a wrapper tha can be used with any data, including objects that implemnt Collections, such as List. A LiveData object is ually stored within a ViewModel object and is accessed via a getter method. 
```
class MyViewModel : ViewModel() {
  // Create a LiveData with a String
  val currentName: MutableLiveData<String> by lazy {
    MutableLiveData<String>()
  }
}
```

Initially, the data in the LiveData object is not set. 
Note: LiveData objects should be stored in a ViewModel
- To avoid boated activites and fragments. Now these UI controllers are responsible for displaying data but not holding data state.
- To decouple LiveData instances from specific activity or fragment instances and allow LiveData objects to survive configuration changes. 

## Observe LiveData objects
In most cases, an app componet's onCreate() method is the right place begin observing LiveData object for the following reason. 
- To ensure the system doesn't make redudndant calls from an activity or fragment's onResume() method
- To ensure that the activity or fragment has data that it can display as soon as it becomes active. As soon as an app component is in teh STARTED state, it receives the most recent value from the LiveData objects it's obserfing this only occurs if the LiveDAta object to be observed has been set.

Generally, liveData delivers updates only when data changes, and only to active observers. An exception this behavior is that observers alos receive an update when they change from an inacgive to an active state. Furthermore, if the observer changes from inactive to active a second time, it only receivese an update if the value has changed isnce the last time it bcame active. 

```
class MyActivity : ApCompatActivity() {
  private val model: MyViewModel by viewModels()
  
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState
    
    // Other code to setup the activity
    
    // create the observer which updates teh UI.
    val nameObserver = Observer<String> { newName -> 
      //update the UI, in this case, a TextView.
      nameTextView.text = newName
  }
  
  // Observe the LiveData, passing in this activity as the LifecycleOwner
  model.currentName.observe(this, nameObserver)
  
}
```

After observe() is called with nameobserver passed as aparameter, onChanged() is immediately invoked providing the most recent value stored in mCurrentName. If the LiveData object hasn't set a value in mCurrentName, onChanged() is not called. 


## Update LiveData objects
LiveDAta has no publicly available methods to update the stored data. The MutableLiveData class expose the setValue and postValue(t) methods publicly and you must use these if you need to edit the vaue stored in a LiveData object. usually MutalbeLiveData is used in the ViewModel and then the ViewModel only exposes immutable LiveData objects oto the observers

After you have set up the observer relationship, you can then update the value of the lvide data object as illustrated by teh following example, which triggers all observers when the user taps a button
```
button.setOnClickListener {
  val anotherName = "John doe"
  model.currentName.setValue(anothername)
}
```

calling setValue inteh example results in teh observers calling their onChanged() methods with the value John doe. The example shows a button press, but setValue() or postValue()  could be called to update name of a variety of reasons, including in response to a network request or a database load completing in all cases the cal lto setValue() or postValue triggers observers and updates the UI.

Note You must call the setValue(T) method to update the LiveData object from the main Thread. If the code is executed in a worker thread, you can use the postValue(t) method instead to updat eh LiveData object.

## Use LiveData with Room
The Room persistence library supports observable queries, which return liveData objects. Observable queries are written as par to f a DAtabase Access Object(dAO).

Room generates all the necessary code to update the LiveDAta object when a database is updated. The generated code runs the query asynchronously on a background thread when needed. This patern is useful for keeping the data displayed in a UI in sync with the data stored in a database. you can read more about Room and DAoos in the Room persistent library guide. 

## LiveData provides support for courtines

## Extend LiveDAta
LiveData considers an observer to be in active stae if the observer's lifecycle is ineither the STARTED or RESUMED states. The following sample code illustrates how to extend the LiveData class
```
class StockLiveData(symbol: String) : LiveData<BigDecimal>() {
  private val stockManger = StockManager(symbol)
  
  private val listener = { price: BigDecimal -> 
    value = price
  }
  
  override fun onActive() {
    stockManager.requestPriceUpdates(listener)
  }
  
  override fun onInactive() {
    stockManager.removeUpdates(listener)
  }
}
```
The implementation of the price listener inthis example include the folowing important methods
- The onActive(0 methods is called when the LiveData object has an active  observer. This means you need to start observing the stock price updates from this method. 

- The onInactive(0 method is calle dhwen the LiveData object doesn't have any active observers. Since no observers are listening, there is o reason to stay connected to the StockManager service

- The setValue(T) method updates teh value of the LiveData instance and notifies any active observers about the change

YOu can use the StockLiveData class as follows
```
public class MyFragment : Fragment() {
  override fun onViewCreated(view:View, savedInstanceState: Bundle?) {
      super.onViewCreated(view, savedInstanceState)
      val myPriceListener: LiveData<BigDecimal> = ...
      
      myPriceListener.obsesrve(viewLifecycleOwner, Observer<BigDecimal>  { price: BigDecimal? -> 
        // update the UI
      }
  }
}
```

The observe() method passes teh LifecycleOwner associated with the fragment's viwe as the first argument. Doing so denotes that this observer is bound to the lifecycle object associated with the owner, meaning
- If the lifecycle object is not in an active state, then the observer isn't called even if the value changes. 
- After the Lifecycle object is destoyed, the observer is automatically removed. 

The fact that LiveData objects are lifcycle-aware means that you can share them between multiple activites, fragments, and services. To keep the eample simple, you can implement the LiveData clasa as  a singleton

```
classs stockLiveData(symbol: String) : LiveData<BigDecimal>() {
  private val stockManager: StockManager = StockManager(symbol)
  
  private val listener =  { price: BigDecimal -> value = price}
  
  override fun onActive() {
    stockManager.requestPriceUpdates(listener)
  }
  
  override fun onInactive() {
    stockManager.removeUpdates(listener)
  }
  
  companion object {
    private lateinit var sInstance: StockLiveData
    
    @MainThread
    fun get(symbol: String: StockLiveData {
      sInstance = if(::sInstance.isInitialized) sInstance else StockLiveDAt
      returns instance of null
    }
  }
}
```

Fragment
```
class MyFragment : fragment() {
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstnaceStat
    StockLiveData.get(symbol.observe(viewLifecycle Owner, Observer<BigDecimal>
    
    })
  }
}
```
