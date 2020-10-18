## Koltin Coroutines with Architecture componetnes
Kotlin coroutines provide an API that enables you to write asynchronous code. With kotlin coroutines, you can define a CoroutineScope which helps you to manage when your coroutines should run. Each asynchronous operation runs within a particular scope. 

Architecture components proide first-class upport for coroutines for logical scope in your app along with an interoperability layer with LiveData. This tpic explains how to use coroutines effectively with Architecture componetns. 

## Add Dependencies

The built-in coroutine scope described in this topic are continaed in the KTX extensions for each corresponding Architecture component. Be sure to ad dthe appropriate dpenedencies when using these scopes. 

- For ViewModelScope, use Androidx.lfiecycle:lifecycle_viewModel-ktx:2.1.0

- LifecycleScope: use Androidx.lifecycle:lifecycle-runtime-ktx:2.2.0-alpha01

- LiveData: use androidx.lifecycle:lifecycle-livedata-ktx:2.2.0-alpha01

## Lifecycle-aware coroutine scopes 
Architecture components defines the following built-in scopes that you can use in your app

## ViewModelScope
A ViewModelScope is defined for each ViewModel in you app. Any coroutine launched in this scope is automatically canceled if the ViewModel is cleared. Coroutines are useful here for when you have work that needs to be done only if the ViewModel is active. For example, if you are computing some data for a layout, you should scope the work to the ViewModel so that if the ViewModel is cleared, the work i scanceled automatically to avoid consuming resources. 

You can access teh CoroutineScope of a ViewModel through the ViewModelScope property of the Viewmodel, as shown below. 
```
class MyViewModel: ViewModel() {
  init {
    viewModelScope.launch {
      // Coroutine that will be canceled when the ViewModel is cleared. 
    }
  }
}

```

## LifecycleScope
A LifecycleScope is defined for each Lifecycle object. Any coroutine launched in this scope is canceled when the Lifecycle is destoryed. You can access the CoroutineScope of the Lifecycle either via lifecycle.coroutineScope or lifecycleOwner.lifecycleScope properties. 

The example below demonstrates how to use lifecycleOwner.lifecycleScope to create precopmuted text asynchronously
```
class MyFragment: Fragment() {
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreted(view, savedInstanceState)
    viewLifecycleOwner.lifecycleScope.launch {
      val params = TextViewCompat.getTextMetricsParams(textView0
      val precomputedtext = withContext(Dispatchres.Default) {
        PrecopmutedTextCompat.create(longTextContent, params)
      }
      TextViewCompat.setPrecomputedText(textView, precopmutedText)
    }
  }
}
```


## Suspend Lifecycle-aware coroutines
Even though the coroutineScope provides a proper way to cancel long-running operations automatically, you migh have other cases where you want to suspend execution of a code block unless the Lifecycle is in a certain state. For example, to run a FragmentTransaction, you must wait until the Lifecycle is at least STARTED. For these cases, Lifecycle provides additional methods: lifecycle.whenCreated, lifecycle.whenStarted, and lifecycle.WhenResumed. 
Any coroutine run inside these blocks is suspended if the Lifecycle isn't at least in the minimal desired stated. 

```
class MyFragment: Fragment {
  init { // Notice that we can safely launch in the contructor of the Fragment.
    lifecycleScope.launch {
      whenStarted {
        // The block inside will run only when Lifecycle is at least STARTED.
        // It  will start executing when fragment is started and can call other supsend methods. 
        loadingView.visibility = View.VISIBLE
        val canAccess = withContext(Dispatchers.IO) {
          checkUserAccounts()
        }
        
        // When checkUserAccounts returns, the next line is automatically suspended if the Lifecycle is not at least STARTED.
        // We could safely run fragment transactions becuase we know the code won't run unless the lifecycle is at least STARTED. 
        loadingView.visibility = View.GONE
        if(canAccess == false) {
          findNavController().popBackStack()
        } else {
          showContent()
        }
      }
    }
  }
  
}
```

If the Lifecycle is destoryed while a coroutine is active via one of the when methods, the coroutine is automatically canceled. In the example below, the finally block runs once the Lifecycle state is Destoryed. 

```
class MyFragment : Fragment {
  init {
    lifecycleScope.launchWhenStarted {
      try {
        // Call some supsned function
      } finally {
        // This line might execute after Lifecycle is DESTORYED.
        if(lifecycle.state >= STARTED) {
          // Here, since we've checked, it is safe to run any 
          // fragment transactions. 
        }
      }
  }
}
```


## Use coroutine with LiveData
When using LiveData, you migh need to calculate values asynchronously. For example, you might want to retrieve a user's prefences and serve them to your UI. In these cases, you can use the liveData builder function to call a suspend function, serving the result as a LiveDAta object. 

In the example below, loadUser() is a suspend function declared elsewhere. use the LiveData builder function to call loadUser() asyncrhonously, and then use emit() to emit the result:

```
val user: LiveData<User> = liveData {
  val data = database.loadUser() // LoadUser is a supsend function.
  emit(data)
}
```

The liveDAta building block sesrves as a structured concurrency primitive between coroutines and LiveData. The code block starts executing when LiveDAta becomes active and is automatically canceled after a configurable timeout when the LiveData beocmes inactive. If it is canceled before completion, it is restarted if the LiveData becomes active again. If it completed successfully in a previou s run, it doesn't restart. Note that it is restarted only if canceled automtaiclly. If the block is canceled for ay other reason it is not restarted. 

You can also emit mutiple values from the block. Each emit() call suspends the execution  of the block until the LiveDAta value is set on the main thread. 
```
val user: LiveData<Result>  = liveData {
  emit(result(Result.loading()) 
  try {
    emit(Result.success(fetchUser()))
  } catch (ioException: Exception) {
    emit(Result.error(ioException))
  }
}
```

You can also combine liveData with Transformations, as shown in the following
```
class MyViewModel : ViewModel() {
  private val userId: LiveData<String> = MutableLiveData()
  val user = userId.switchMap { id -> 
    liveData(context = viewModelScope.coroutineContext + Dispatchers.IO) {
      emit(database.loadUserById(id)) 
    }
  }
}
```

You can emit multiple values from a LiveData by calling the emitSource() function whenever you want to emit a new value. Note that each call to emit(0 or emitSource(0 removes the previously added source. 
```
class UserDao: Dao {
    @Query("SELECT * FROM User WHERE id = :id")
    fun getUser(id: String): LiveData<User>
}

class MyRepository {
    fun getUser(id: String) = liveData<User> {
        val disposable = emitSource(
            userDao.getUser(id).map {
                Result.loading(it)
            }
        )
        try {
            val user = webservice.fetchUser(id)
            // Stop the previous emission to avoid dispatching the updated user
            // as `loading`.
            disposable.dispose()
            // Update the database.
            userDao.insert(user)
            // Re-establish the emission with success type.
            emitSource(
                userDao.getUser(id).map {
                    Result.success(it)
                }
            )
        } catch(exception: IOException) {
            // Any call to `emit` disposes the previous one automatically so we don't
            // need to dispose it here as we didn't get an updated value.
            emitSource(
                userDao.getUser(id).map {
                    Result.error(exception, it)
                }
            )
        }
    }
}
```
