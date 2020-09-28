# Binding Class
DataBinding generates binding class that are used to access the layout's variables and views. The generated binding class links the layout variables with the views within the layout. The name of the of the binding class can be ccustomized. All generated binding classes inherit from the ViewDataBinding class. A binding class is generated for each layout file. By default, the name of the class is bassed on the name of the layout file, converting it to Pascal case and adding binding suffix to it. 

Ex 
activity_main.xml becomes ActivityMainBinding

This class holds all the bindings from the lyout properties to the layouts views and knows how to assign values for the binding expression.

## Create a binding object
The binding object is created immediately after inflating the layout to ensure that the view hierarchy isn't modified before it binds to the views with expressions within the layout. You can inflate the view hierarchy and bind the object to it by using the inflate() method of the binding class. 

fragment
```
override fun onCreateView(
  inflater: LayoutInflater,
  container, ViewGroup?
  savedInstanceState: Bundle?
): View? {
  val binding = DataBindingUtil.inflate(inflater, R.layout.my_fragment, container, false)
  
  return binding.root
}
```

## Views with IDs
The DataBinding library creates immutable filed in the binding class for each view that has an ID in the layout. The library extracts the views including th e IDs from the view hierarchy in a single pass. This mechanims can be faster than calling findViewById.  IDs aren't as necessary as they are without data binding, but there are still some instances where access to views is stil nedessary from code. 

## Variables
The data bindinding library generates accessor methods for each variable declared in the layout.

## ViewSubs
Unlike normal views, ViewSub objeccts start off as an invisible view. When they either are made visible or are explicitly told to inflate, they replace themselves in the layout by inflating another layout. 

Becasue the ViewSub essentially disappears from the view hierarchy, the view in the binding object must also disappear to allow to bclaimed by garbage collection. Becuase the views are final, a ViewStubProxy object takes the place of the ViewStub in the gernerated binding class, giving you access to the ViewStub wehn it exists and also access to the inflated view hierarchy when the viewStub has been infalted. 

When infallting another alyout, a binding must be established for the new layout. Therefore, the ViewStubProxy must listen to the ViewStub OnInflaterListener and establish the binding when required. Since only one listener can exist at a ggiven time, the ViewStubProxy allow s you to set an OnInflaterListener, which it calls after establishing the binding. 

## Immediate Binding  
When a variable or observable object changes, the binding is scheduled to change before the next frame. There are times, however, when binding must be executed immediately. To force execution use the executePendingBindings() method

## Advanced Binding
Dynaic Variables
At times, the specific binding class isn't known. For example a RecyclerView.Adapter operating against arbitrary layouts doesn' tknow the specific binding class. It still must assign the binding value during the call to the onBindViewHolder()

In the following example, all layouts that the RecyclerView binds to have an item variable. The BindingHolder object has a getBinding() method returning the ViewDataBinding base class. 

```
override fun onBindViewHolder(holder: BindingHolder, position: Int) {
  item: T = items.get(position)
  holder.binding.setVariable(BR.item, item)
  holder.binding.executePendingBindings()
}
```

## Background Thread
You can change your data modle in a background thread as long as it isn't a collection. Data binding localizes each variable / field during evaluation to avoid any concurrency issues. 

