## ViewModelScope
A ViewModelScope is defined for each ViewModel in you app. Any coroutine launched in this scope is automatically canceled if the ViewModel is cleared. Coroutines are useful here for when you have work that needs to be done only if the ViewModel is active. For example, if you are computing some data for a layout, you should scope the work to the ViewModel so that if the ViewModel is cleared, the work is canceled automatically to avoid consuming resources. 

You can access the CoroutineScope of a ViewModel through the ViewModelScope property of the Viewmodel, as shown below. 
```
class MyViewModel: ViewModel() {
  init {
    viewModelScope.launch {
      // Coroutine that will be canceled when the ViewModel is cleared. 
    }
  }
}

```
