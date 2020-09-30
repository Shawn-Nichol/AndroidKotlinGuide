# How to create LiveData
## 1) Create an instance of LiveData to hold a certain type of data. This is usually done within your ViewModel class. 
```
class MyViewModel() : ViewModel() {
  // Allows view model to set values
  val _counter: MutableLiveData<Int> = MutableLiveData()
  // You'll observe this LiveData as it can't be update from the UI. 
  val counter: LiveData<Int> = _counter
}
```

## 2) Create an Observer object that defines the onChanged(), which controls what happens when the LiveData object's data changes. You usually create an Observer object in a UI controller, such as activity or fragment. 

Activity or fragment
```
class MainActivity : AppCompatActivity() {
  ...

  myViewModel.counter.observe(this, Observe {
    Log.i(TAG, "LiveData changed, $it)
  })
}
```

## 3) Update you with DataBinding. 
Use the two way databinding to update Views with DataBinding
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="viewModel"
            type="com.example.practice.MyViewModel" />
    </data>
    
    ...
            <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{Integer.toString(viewModel.counter)}" />
    
```





