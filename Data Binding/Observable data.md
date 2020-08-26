# Observable data objects
Observability referes to the capability of an object to notify others about changes in its data. The data binding library allows you to make objects field or collections observable.

Any plain-old object can be used for data binding, but modifying the object doesn't automatically  cause the UI to update. Data binding can be used to give your data objects the ability to notify other objects, known as listeners, when its data changes. There are three differen types of observable class objects, fields, and collections

When one of thes objservable data objects is bound to the UI and a property of the data object changes, the UI is updated automatically. 


## Livedata example


Activity
```
val _counter: MutableLiveData<Int> = MutableLiveData()
val counter: LiveData<int> = _counter

fun buttonClick() {
  // add one to the _counter
  _counter.value = (counter.value ?:0) + 1
}
```

XML file
```
<layout .../>
  <data>
    <variable name="binding" type=".example.MyActivity"/>
  </data>
  
  <constraint ...>
    <TextView 
      ...
      // Grabs the info from live data and updates automatically. 
      android:text="@{Integer.toString(mybinding.counter)}"
  </constaint>

```
