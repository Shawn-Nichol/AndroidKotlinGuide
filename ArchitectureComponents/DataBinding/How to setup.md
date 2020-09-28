# How to setup 

## Gradle file
Enable data binding in the build.gradle file

```
    // Enables data binding.
    dataBinding {
        enabled = true
    }
```

## Activity file
- Create a binding variable. 
- Link the layout to the binding variable.
- Add the expression to the binding variable, this is need or the onClick won't work

```
class MainActivity : AppcompatActivity() {
  // You may need to rebuild the code in order for the ActivityMainBinding activity to be created. 
  // Binding class gets its name from the layout file ex. activity_main become ActivityMainBinding. 
  private lateinit var = binding: ActivityMainBinding
  
  Override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstance)
    
    // Link layout to databinding variable
    binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
    // add XML variable name expression to databinding with context
    binding.mybinding = this
  }
  
  // This method must be public or the XML expression won't see the method. 
  val fun myClick() {
    // do Something
  }
}

```

## XML file
- In the layout file Convert to data bidning layout
- In the <data> tags, Write a layout expression with the vaiable tags. 
- add method reference to a view

```
<layout
...> 
  <data>
    <variable
      name="myBinding" // You can name it what ever you want. 
      type="com.example.MainActivity" 
    />
  </data>


  <Button
    android:id="@id/myButton"
    ...
    android:onClick="@{() -> myBinding.myClick()}"
  />

</layout>

```
