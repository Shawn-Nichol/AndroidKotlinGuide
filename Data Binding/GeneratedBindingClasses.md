The Data binding Library generates binding classes that are used to acceess the layout's variables and views. This page shows how to create and customize generated binding classes

The generated binding class links the layout variables with the view within the layout. The name and package of the binding class can be customized. All generated binding classes inherit from the ViewDataBinding class. 

A binding class is generated for each layout file. By default, the name of the class is based on the name of the layout file, convertin it to Pascal case and adding the Binding suffix to it. The above layout filename is activity-main.xml so the corresponding generated class is ActivityMainBinding. THis class holds all the bindings from the alyout properties to the layotus views and knows how to assign values for the binding expressions

## Create a binding object 
The binding object is created immediately after inflating the layout to ensure that the view hierarchy isn't modified before it binds to the views with expression within the layout. The most common method to bind the object to a layout is to use the static methods on the binding class. You can inflate the view heirarcy and bind the object to it by using the inflate() method of the binding class. 

```
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  val binding: MyLayoutBinding = MyLayoutBinding.inflate(layoutInflater)
  setContentView(binding.root)
  
}
```

There is an alternative version of the infalte() that takes a ViewGroup object in addtion to the LayoutInflater object. 
```
val binding: MyLayoutBinding = MyLayoutBinding.inflate(getLayoutInflater(), ViewGroup, false)
```

Sometimes the binding type cannot be known in advance. In such cases, the binding can be created using the DataBindingUtil class

```
val viewRoot = LayoutINflater.from(this).inflate(LayoutId, parent, attachToParent)
val binding:  ViewDataBinding? = DataBindingUtil.bind(viewRoot)
```

If you are using data binding items inside a fragment, ListVIew or RecyclerView adapter, you may prefer to use the inflate() of the binding class or the DataBindingUtil class. 

```
val listItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)
val listItemBinding = DataBindingUtil.inflate(layoutINflater, R.layout.list_item, viewGroup, false)

## Views with ID
```
