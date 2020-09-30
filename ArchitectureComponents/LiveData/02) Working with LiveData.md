## Observe LiveData objects
In most cases, an app componet's onCreate() method is the right place to begin observing LiveData objects for the following reasons.
- To ensure the systme doesn't make redudndant calls from an activity or fragment's onResume() method.
- To ensure that the activity or fragment has data that it can display as soon as it becomes active. As soon as an app component is in the STARTED state, it receives the most recent value from the LiveData objects it's observing this only occurs if the LiveData object to be observed has been set. 

Generally, LiveData delivers updates only when data changes, and only to active observers. An exception of this behavior is that observses also receive an update when they change from an inactive to an active state. Furthermore, if the observer changes from inactive to active a second time, it only receives an update if the value has changed since the last time it became active. 
```
class MyActivity : ApCompatActivity() {
  private val model: MyViewModel by viewModels()
  
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState
    ...    
    // create the observer which updates the UI.
    val nameObserver = Observer<String> { newName -> 
      //update the UI, in this case, a TextView.
      nameTextView.text = newName
  }
  
  // Observe the LiveData, passing in this activity as the LifecycleOwner
  model.currentName.observe(this, nameObserver)
  
}
```
After observe() is called with a name observer passed as  a parameter, onChanged() is immediately invoked providing the most recent value stored in mCurrentName. If the LiveData object hasn't set a value in mCurrentName, onChanged() is not called. 

## Update LiveData objects
LiveData has no publicly available methods to update the stored data. The MutableLiveData class exposes the setValue and postValue(t)methods publicly and you must use these if you need to edit the value stored in a LiveData object. Usually MutableLiveData is used in the ViewModel and then the ViewModel only exposes immutale LiveData objects to the observers. 

After you have set up the observer relationship, you can then update the value of the LiveData object as illustratedd by the following example, which triggers all observers when the user taps a button.
```
button.setOnClickListener {
  val anotherName = "John doe"
  model.currentName.setValue(anothername)
}
```
Calling setValue in the example results in the observers calling their onChanged() methods with the value John Doe. The example shows a button press, but setValue() or postValue() could be called to update name for a variety of reasons, including in response to a network request or a database load completing in all cases the the setValue() and postValue triggers observers and update the UI.

Note you must call the setValue(T) method to update the LiveData object fromm the MainThread. If the code is executed in a worker thread, you can use th postValue(T) method instead to update the LiveData object. 

## Use LiveData with Room
The Room persistence library supports observable queries, which return LiveData objects. Observable queries are written as part of a Database Access Object(DAO).

Room generates all teh necessary code to update the LiveData object when a database is updated. The generated code runs the query asynchronously on a background thread when needed. This pattern is useful for keeping the data displayed in a UI, insync with data stored in a database.

## Extend LiveDAta
LiveData considers an observer to be in active state if the observer's lifecycle is in either the STARTED or RESUMED state. 
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
- The onActive() method is called when the LiveData object has an active observer. This means you need to start observing the stock price updates from this method. 

- The onInactive() method is called when the LiveData object doesn't have any active observers. Since no observers are listening, there is no reason to stay connected to the StockManager service.

- The setValue(T) method updates the value of the LiveData instance and notifies any active observers about the change.

You can use the Stock LiveData class as follows.
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
The observe() method passes the LifecycleOwner associated with the fragment's view as the first argument. Doing so denotes that this observer is bound to the lifecycle object associated with the owner, meaning.
- If the Lifecycle object is not in an active state, then the observer isn't called even if the value changes. 
- After the Lifecycle object is destroyed, the observer is automatically removed. 

The fact that LiveData objects are Lifecycle-aware means that you can share them between multiple activites, fragments, and services. To keep the example simple, you can implement the LiveData class as a a signleton.

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
The observe() method passes the LifecycleOwner associated with the fragment's view as the first arguemnt. Doing so denotes that this observer is bound to the Lifecycle object associated with the owner. 
- If the Lifecycle object is not in a active state, then the observer isn't called even if the value changes. 
- After the Lifecycle boject is destoyed, the observer is automatically removed. 

The fact that LiveData objects are lifecycle-aware means that you can share them between multiple activites, fragments and servies. To keep the example simple, you can implement the LiveData class as a Singleto as follows. 
```
class StockLiveData(symbol: String) : LiveData<BigDcimal>() {
  private val stockManager: StockManager = StockManager(symbol)
  
  private val listener = { price: BigDecimal -> 
    value = price
  }
  
  override fun onActive() {
    stockManager.requestPriceUpdates(listener)
  }
  
  override fun onInactive() {
    stockManager.removeUpdates(listener)
  }
  
  companion object {
    private lateinit var sInstnce: StockLiveData
    
    @Mainthread
    fun get(symbol: String): StockLiveData {
      sInstance = if(::sInstanace.isInitialized) sInstance else StockLiveData(symbol)
      return sInstnace
    }
  }
}

class MyFragment : Fragment() {
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    StockLiveData.get(symbol).observe(viewLifecycleOwner, Observer<BigDecimal>
    // Update the UI
    })
  }
}
```
Multiple fragments and activities can observe teh MyPRiceListener instance. LiveData only connets to the systmeservice if one or more of them is visible and active. 


## Transform LiveData
You may want to makes changes to the value stored in a LiveData object before dispatching it to the observers, or you may need to return a different LiveData instance based on the value of another one. The Lifecycle package provides the Transiformation class which includes help methos that support these scenarios. 

Transformation.map()
Applies a function on the value stored in the LiveData Object, and propagates the result downstream. 

```
userLiveData: LiveData<User> = UserLiveData()
val UserName: LiveData<string> = Transformations.map(userLiveData) {
  user -> "${user.name} ${user.lastname}"
}
```
Transformation.switchmap()
Similar to map(), applies a function to the value stored in the LiveData object and unwraps and dispatches the result downstream. The function passed to switchMap() must return a LiveData object, as illustrated by the following examp.e. 

```
private fun getUser(id: Sttring): LiveData<User> {
  ...
}
  val userId: LiveData<String> = ...
  val user = Transformation.switchMap(userId) { id-> getUser(id) }
```

You can use transformation methods to carry information across the observer's lifecycle. The transformations aren't calculated unless an observer is watching the return LiveDAta object. Because the transforamtions are calculated lazily, lifecycle-related behavior is implicitly passed down without reuiring additional explicit calls oor dpeendencies. If you think you need a Lifecycle object inside a Veiw ogject, transormation is proably a better solultion. For example ssume that you havea  UI component that accepts an addres and returnts the postal code for that address. For example assume that you have a UI componet that accepts an address and returns the postal code for that address.
```
class MyViewModel(private val repository: PostalCodeRepository) : ViewModel() {
  private fun getPostalCode(address: String) : LiveData<String> {
    return respository.getPostCode(address)
  }
}
```

The UI component then needs to unregister from teh previous LiveData object an register to the new innstance each time it calls getPostalCode(). In addition, if the UI component is recreated, it triggers another call to the repository.getPostCode() method instead of using the previous call's result. 

```
class MyViewModel(private val respository: PostalCodeRepository) : ViewModel {
  private val addressInput = MutableLiveData<String>()
  val postalCode: LiveData<String> = Transofrmations.switchMap(addressInput) {
    address -> respository.getPostCode(address) }
    
    private fun setInput(address: String) {
      addressInput.value = address
    }
}
```

In this case, the postalCode field is defined as a transformation of the addressInput. As long as your app has an active observer associated with the postalCode field, the field's value is recalculated and retrieved whenever addressInput changes. 

This mechanism allows lower levels of the app to create LiveData objects that are laily calculated on demant. A ViewModel object can easily obtain references to LiveData objects an dthen define transformation rules on top of them. 

## Create new tranformatoins
There are a dozens of different transformation that may be useful in your app, but they aren't provided by default. To implement your own transformation you can use the MediatorLiveData class, which listens to other LiveData objects and processes events emitted by them. Mediaor LiveData correctly propagates its state to the source LiveData object. To learn more about this pattern, see the reference documentation of the Transformations class. 

## merge multiple LiveData sources
MediatorLiveData is a subclass of LiveData that allows you to merge multiple LiveData sources. Observers of MediatorLiveData are then triggered whever any of the orignial LiveData source object change

For example, if you have a liveData object in your UI that can be updated from a local database or a network, then you can add the following scource of the MediatorLivedata object
 - A liveData object associated with the data stored in the database. 
 - A liveData object associated with the data accessed from the network. 
