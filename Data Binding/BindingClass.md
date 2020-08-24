# Binding Class
A binding class is generated for each layout file. By default, the anme of the class is based on the name of the layout file, converting it to Pascal case and adding the binding suffix to it. The class holds all the bindings from the layout properties to the layout's views and knows how to assign values for binding expressions. The recommended method to create the binding is to do it while inflating the laout

```
fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  val binding: ActivityMainBinding = DataBindingUtil.sestContentView(this, R.layout.activity_main)
}
```

Layoutinflater, alternative
```
val binding: ActivityMainBinding = ActivityMainBinding.infalter(getLayoutInflater())
```

