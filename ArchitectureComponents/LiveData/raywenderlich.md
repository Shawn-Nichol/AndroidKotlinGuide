## LifeCycleawareComponents
AppCompatActiivty and Fragment are some of the lifecycle owner objects in teh AndroidFramewokrk. Lifecycleaware components are classes or objects that react based on lifecycle methodds of lifecycleowner. They react to changes in the lifecycle status of another component like Activity or fragment. You can relate it with registering and unregistering locatino updates in activity's onstart and onstop lifecycle methods

LiveData and MutalbeLiveData
LiveData is a Lifecycleaware component and an observable data holder class before the indtroduction of LiveData, you might have used the callback mechanism to update your UI . However, callback has its own set of issues like manual lhandlin gof lifecylce, memory leaks, keeping up to date correct data to name a few. 

Initialize Update, Observe LiveData
LifeDAta is a wrapper on an object which can be observed from any UI component via getter method. LiveDAta is generally initialized in ViewModel and update on some maunl or automic actions. 

```
1
val liveData: MutableLiveData<Any>()
2
liveData.value = "hello"
3
liveData.observe(this, Observe {
  // update UI. 
})
```

1 LiveData is alwasy intiailized with on of its child class constructors with exception of Transformations and MediaotrLiveData. 
2. LiveData declared a s val can't change, but you can alwasy update the value of LiveData becuase it is MutableLive.data. In change in the value of liveData notifies all of its active observers. 
3. LiveDAta is alwasy observed inside a UI liifecycle owner, which can be an activity or Fragment. you recieve the latest value of liveDAta in n observer as an implicit parameter it. 

Synchronus vs Asynchronous Update
LiveData doesn't have public methods to update its value. hence you'll use MutableLiveDAta to update teh value inside LiveData with the help of the following 2 options. 

1. SetValue: sets the value instantly. This is synchronous update wher main thread calls setValue. With Kotlin's property access syntax, you'll often use value instead of setValue. 
2. postValue: Asynchrouns updating, means observer doesn't receive instant update rather receives update when UI thread is active. When you call postvalue more than one time from backgroun thread, livedat dispatches only the latest value to the downstream. Being asyncrounous, this does not gaurantee instant update tot he observer. 
