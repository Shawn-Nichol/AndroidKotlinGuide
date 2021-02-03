# Mockito
- Step one: Configure what you're are going to test
- Step two: Execute the method that you want to test
- Step three: Vefiry the result by checking the state of the object under test. This is called state verification or black-box testing. 

## Setting up Mockito
add dependencies 
```
dependencies {
  testImplementation 'com.nhaaramn.mockitokotlin2:mockito-kotlin:2.1.0'
}
```
Mockito-Kotlin is a wrapper library around Mockito. It provides top-level functiosn to allow for a more kotlin like approach and also solves a few issues with using the Mockito Java library in kotlin. 

Kotlin classes and methods are final by default. mockito won't work with the final classes methods out of the box. To fix this you have the following options
- Use a mock-maker-inline extension: to allow Mockito mock final classes method
- Add the open keyword to classes and methods that you'll mock
- Create an interface and have the class implement the interface. Then just mock the interface.


## Stubbing
Uses a fake collaborator object that always returns the same results.
```
whenever(object.method(arguement matcher))).thenReturn(results)
whenever(question.answer(anyString())).thenReturn(true)
```

## Verfication

Modes: tiems(), are: never(), atLeast(), atMost(). Other areguemtn matchers eq(), are same(), any()

You can very mupltiple object in a predfined order
```
inOrder(sharedPreferencesEditor) {
  verify(sharedPReferencesEditor).putInt(any(), eq(score))
  verify(sharedPreferencesEditor).apply()
}
```

## Spying
Lets you call the methods of real object, while also tracking every interaction, just as you would do with a mock. When setting up spies, you need to use `doReturn/whenever/method` to stub a method.

## Mock-makerinlline extension
Allows Mockito mock finals classes methods.  

implement </br>
Create a resources directory under app/src/test/mockito-extensions and create a file called org.mockito.pluginsMockMaker with the following code
```
mock-maker-inline
```
## Testing ViewModel LiveData
Add dependencies
```
implementation 'androidx.lifecycle:lifecycle-extensions:2.0.0'
testImplementation 'androidx.arch.core:core-testing:2.0.1'
```
Use `@get:Rule`. This is a test rule, A test rule is a tool to change the way tests run, somtimes adding additional checks or running code before and after your tests. Android architecture componets use a bakcground executor that is asynchronous. `InstantTaskExecutorRule` is a rule that swaps out that executor and replaces it with syncronous one. This will ensure when using Livedata with ViewModel it all runs syncronously in the test. 


## Annotation
`@RunWith(MockitoJUnitRunner::class)`: instructs that you are going to write tests using Mockito. Now you can annotate using @Mock every preoopty that you'll later use as mocks. </br>
`@Mock`
