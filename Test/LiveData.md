# How to test live Data

## TaskExecutor Rule
InstnatTaskExecutorRUle is a JUnit Rule. When you use it withthe @get:Rule annotation, it causes some code in the InstantTaskExecutorRule class to be run before and after the test. This rule runs all Architecture components-related background jobs in the same thread so that the test result happen synchronously, and in a repeatable order.  When you write tests that include testing LiveData, use this rule.


Depenedency
```
testImplementation "androidx.arch.core:core-testing:$archTestingVersion"
```

Enable rule
```
@get:Rule
var instnatExecutorRule = InstantExecutorRule()
```

## Observer live data
Observer the LiveData with a Lifecycleowener, use the `observerForever`


```
@Mock
private lateinit var myObserver: Observer<Int>


@Mock
private lateinit var viewModel: MyViewModel()

@Before
fun setup() {
  viewModel.myLiveData.observeForever(myObserver)
}



@Test
fun myTest() {
  viewModel.changeMyLiveData() 
  
  // verify the observer sees a change in the livedata value
  veriy(myObserver).onchanged(any())
  
  // Check the myLiveData value to ensure it equals the expected value. 
  Assert.assertEquals(5, viewModel.myLiveData.value)
}

```



