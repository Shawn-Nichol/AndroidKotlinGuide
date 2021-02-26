# How to create a ViewModel

## 1) Add dependdencies
- Select current dependency
https://developer.android.com/jetpack/androidx/releases/lifecycle
\
- Open Build Gradle (module:app) and implement the dependency
```
//ViewModel
implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.2.0'
```
Sync Gradle.

## 2) Create a ViewModel class
Create a new ViewModel class, all data and methods you want to be called from the fragment need to public

onClear()
is called when the ViewModel is no longer needed and will be destroyed. 
```
class ViewModel : ViewModel() {
  var counter = 0
  
  // runs on startup
  init {
    
  }
  
  override fun onClear() {
    super.onClear()
  }
  
  fun counterUp() {
    counter++
  }
  
  fun counterDown() {
    counter--
  }
  
}
```

3) Create a View Model class
Use a ViewModelProvider to create ViewModel objects rather than directly instantiating an instance of ViewModel.

ViewModelProvider resturns an existing ViewModel if one exists, or it creates a new one if it does not already exist, ViewModelProvider creates ViewModel for the scope. Initialize the ViewModel, using ViewModelProvider.get() method, and pass in the associated context and the ViewModel class.
```
val viewModel = ViewModelProvider(this).get(MyViewModel::class.java)
```

# ViewModelFactory
A ViewModel factory is used to create ViewModel and pass in data. 

## 1) Create a ViewModelFactory
Create a ViewModelFacotory class, and pass in the starting data. 
```
class MyFragmentViewModelFacotry(private val number: Int) : ViewModelProvider.Factory {
  override fun<T : ViewModel?> create(modelClass: Class<T>): T {
    if(modelClass.isAssignableFrom(MyFragmentViewModel::class.java)) {
      return MyFragmentViewModel(number) as T
    }
    throw IllegalArgumentException("Unknown ViewModel class")
  } 
}
```

## 2) Create a ViewModel class
Create a new ViewModel class, all data and methods you want to be called from the fragment need to public

onClear()
is called when the ViewModel is no longer needed and will be destroyed. 
```
class ViewModel(savedCounter: Int) : ViewModel() {
  var counter = savedCounter
  
  // runs on startup
  init {
    
  }
  
  override fun onClear() {
    super.onClear()
  }
  
  fun counterUp() {
    counter++
  }
  
  fun counterDown() {
    counter--
  }
  
}
```

## 3) Start ViewModel
Create a ViewModelFactory object and passing in the desired arguments. Add The ViewModelFactory object as an argument for the ViewModelProvider. 
```
val viewModelFactory: MyViewModelFactory = MyViewModelFactory(8)
val viewModel =  ViewModelProvider(this, viewModelFactory).get(MyViewModel::class.java)
```
```
