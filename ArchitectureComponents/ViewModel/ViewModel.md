# ViewModel Overview
The ViewModel calss is designed to store and manage UI-elated data in a lifecycle conscious way. The ViewModel class allows data to survive configuration chanages such as screen rotations

The android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-createa UI controller in response to certain user actions or device events that are completely out of your control if the system destroys or re-creates a UI Controller, any transient UI-related  data you store in them is created for a configuration change, the new activity has to re-fetch the list of users. For simple data, the activity can use the onSaveInstanceState() method and restore its data from the bundle in onCreate(), but this approach is only suitable for small amount sof data that can be serialized then deserialized, not for poetntially lare amounts of data like a list of users or bitmaps. 

Another problem is that UI controllers frequently need to make sasynchronous calls that may take some time to treturn. The UI controllers frequetnly need to makes asynchronous calls that may take some time to return. The UI controller needs to manage these calls and ensure the system cleans them up after it's destoryed to avoid potentail memory leaks. This management requires a lot of maintance, and in the case where the object is re-created for a configuration change, it's a waste of resources since the object may have to reissue calls it has already made. 

UI controllers such as activites and fragments are primarily intended to displays  UI data, react to user actions, or handle operating system communicatinos, such as permission requests. Requiring UI controllers to also be responsilbble for loading data from a database or network adds bloat to the class. Assigning exessive responsibility to UI controllers can result in a single class that tries to handle all of an app's work by itself, instead od delegating work to other classess. Assigning exceessive respo9nsibility to the UI controlers in this way also makes teseting a lot harder

It's easier  and more effiecient to separate out view data ownership from UI control logic. 

## Implement a ViewModel
Architecture COmpoonents provides ViewModel helper class for the UI controller that is responsible for preparing data for the UI. ViewModel objects are automatically retained during configuration changes so that data they hold is immediatelly avaiable to the next activity or fragment instance. For example, if you need to display a list of users in your app, make sure to assign responsibility to acquire and keep the list of users to a ViewModel, instead of an activity or fragment, 

```
class MyViewModel : ViewModel() {
  private val users: MutableLiveData<List<User>> by lazy { 
    MutableLiveData().also {
      loadUser
    }
  }
  
  fun getUsers(): LiveData<List<User>> {
    return users
  }
  
  private fun loadUsers() {
    // do asynchrouns operation to fetch users. 
  }
}

```

You can then access a list from an activity
```
class MainActivity : AppCompatActivity() {
  overridde fun onCreatte(savedInstanceState: Bundle?) {
    // Create aa ViewModel the first time the sytem calls an activity's onCreate
    // Re-createed activities receive the same MyViewModel instnace createed 
    
    // Use the 'by viewModels()' kotlin property delegate for the activity-ktx artifact
    val model: MyViewModel by viewModel()
    model.getUsers().observe(this, Observer<List<User>> { users -> 
      // update UI
    }
  }
}
```

If the activity is re-created, it receives the same MyViewModel instance that was created by the first activity. When the owner activity is finished, the framework calls the viewmodel object onCleared()

ViewModel objects are designed to outlive specific instantiations of views or LifecycleOwners. This design also means you can write tests to cover a ViewModel more easily as it doesn't know about view and Lifecycle objects. ViewModel objects can contain LifecycleObservers, such as LiveData objects. However ViewModel objects must never observe changes to lifecycle-aware observables, such as LiveData objects. If the ViewModel needs the application context for example to find a system service, it can extend the AndroidViewModel class and have constructor that receives the Application in the constructor, since application class extends context.


## The Lifecycle of a ViewModel
ViewModel objects are scoped to the Lifecycle passed to the ViewModelProvider when getting the ViewModel. The ViewModel remains in memory until the Lifecycle it's scoped to goes away permanently in the case of an activity, when it finishes, while in the case of a fragment when it's detached. 

You usually request a ViewModel the first time the sytem calls an activity object onCreate() method. The system may call onCreate() several time throughout the life of an activity, such as when a device screen is rotated. The ViewModel exists from when you first request a ViewModel unil the activity is finshed and destroyed. 

## Share data between fragments

```
class sharedViewModel : ViewModel() {
  val selected = MutableLiveData<Item>()
  
  fun selected(item: Item) {
    selected.value = item
  }
}

class FragmentOne : Fragemnt() {
  private lateinit var itemSelector: Selector
  
  // Use the 'by activityViewModels()' kotlin property delegate from teh fragemnt-ktx artifact
  prviate val model: SharedViewModel by activityViewModels()
  
  override fun onViewCreated(view: View, saveeInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState) 
    itemSelector.setOnClickListener { item -> 
      // update the UI
    }
  }
}

class DetailFragment : Fragment() {
  private val model: SharedViewModel by activityViewModels()
  
  override onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, saveInstanceState: Bundle?) {
      model.selected.observe(viewLifecycleOwner, Observer<Item> { item -> 
      // updatte the UI
    })
    }
  }
}
```

Notice that both fragments retrieve the activity that contains them. That way, when the fragments each get teh ViewModelProvider, they receive the same sharedViewModel instance, which scoped to this acitivty. 

This approach offers the following
- The activity does not need to do anything, or know anything about this communication
- Fragmetns don't need to do anything, or know anything about this communication.
- Each fragment has its own lifecycle and is not affected by the lifecyclel of the other one. If one fragment replacses the other one, the uI continue sto worko withou any probem. 

## Replacing loaders with ViewModel
Loader classes like CursorLoader are frequently used to keep the data in an app's UI in synce with a daatabase. You an use ViewModel with a few other classes, to replace the loader. using ViewModel separates your UI controller from the data-loading operation, which means you have fewer strong references between classes. 

in one common approach to using loaders, an app might use a CursorLoader to observe the contents of a database. When a value in the database changes ,the loader automatically triggers a reload of the data nd updates the UI. ViewModel works with Room and LiveData to replace the loader. The ViewModel ensures that the data sruvives a device configuration change. Room informs your LiveData when the database changes, and the LiveData, in turn updates your UI with the revised data. 
