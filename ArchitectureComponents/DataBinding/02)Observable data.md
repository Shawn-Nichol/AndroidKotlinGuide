# Observable data objects
Observability referes to the capability of an object to notify others about changes in its data. The data binding library allows you to make objects field or collections observable.

Any plain-old object can be used for data binding, but modifying the object doesn't automatically cause the UI to update. Data binding can be used to give your data objects the ability to notify other objects, known as listeners, when its data changes. There are three differen types of observable class objects, fields, and collections

When one of thes observable data objects is bound to the UI and a property of the data object changes, the UI is updated automatically. 


## Livedata example

ViewModel
```
class MySharedViewModel(savedNum: Int) : ViewModel() {
    val _counter: MutableLiveData<Int> = MutableLiveData()
    val counter: LiveData<Int> = _counter

    fun counterUp() {
        Log.i(TAG, "CounterUp")
        _counter.value = (_counter.value ?: 0) + 1
    }

    fun counterDown() {
        _counter.value = (_counter.value ?: 0) - 1

    }
```

XML file
```

<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="binding"
            type="com.example.practice.OneFragment"/>

        <variable
            name="viewModel"
            type="com.example.practice.MySharedViewModel" />
    </data>

    ...
    <TextView
    android:id="@+id/tv_vm_counter"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{Integer.toString(viewModel.counter)}" />
    
</layout>
```
