Android studio supports many of th e editing features for data binding code. For example it supports the following features for data binding expressions: 
- Syntax highlighting
- Flagging of expression language syntax errors
-  XML code completion
- References, including navigation 

## Layout and Binding expression
The expression language allows you to write expressions that handle events dispatched  by the views. The data Binding Library automatically generates the classes required to bind the views in the layout with your data objects. 

Data binding layout files are slightly different and start with a root tag of layout followed by a data eleemnt and a view root element. This view element is what your root would be in a non-binding layout file. The follwoing code shows a sample

```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
   </LinearLayout>
</layout>
```

The user variable within data describes a property taht may be used within this layout. 

Expression within the layout are written in the atttribute properties using the "@{}" syntax. 

## Data Object
Dat object

```
  data class User(val firstName: String, val lastName: String)
```

This type of object has data that never changes. It is common in application sto have data that is read once and never changes therafter. It is also possible to use an object that follows a set of convertions, suhc as the usage of accessor methods in Java, as shown in the following exmaple 


From the perspective of data binding, these tow classe are equilvant. The expresssion @{user.firstName}
used for th android text attributes access the firstName field in the former class and the getFirstNmae9) method in the latter class. Alternatively it is also resolved to firstName if that method exists. 

## Binding data. 
A binding class is generated for each layout file. By default, the name of the class is based on the name of the layout file, converting it to Pascal case and adding the Binding suffix to it. The above layout filenamee is acitivty_main.xml. So the corresponding generated class is ActivityMainBinding. This class holds all the binding from the layout properties to the layout's views and knows how to assign values for the binding expressions. The recommmended  method to create the bindnign is to do it while inflating the layout.

```
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  
  val binding: ActivityMainBinding = DataBindingUtil.setContentView(this, R.layout.main_activity)
  binding.user = User("Test", "User")
}
```

At runtime the app displays the Text user in the UI. Alternatively, you can get the view using a LayoutInflater.
```
val binding: ActivityMain = ActivityMainBinding.inflate(getLayoutInflater())
```

If you are using data binding items inside a Fragment, ListView, or RecyclerView adapter, you may prefer to use the inflate() methods of the bindings classes or the DataBindingUtil class

```
val ListItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)
// or 
val listItemBinding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup)
```
