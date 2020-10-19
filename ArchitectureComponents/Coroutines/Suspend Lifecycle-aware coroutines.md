## Suspend Lifecycle-aware coroutines
Even though the coroutineScope provides a proper way to cancel long-running operations automatically, you might have other cases where you want to suspend execution of a code block unless the Lifecycle is in a certain state. For example, to run a FragmentTransaction, you must wait until the Lifecycle is at least STARTED. For these cases, Lifecycle provides additional methods: lifecycle.whenCreated, lifecycle.whenStarted, and lifecycle.WhenResumed. 
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
