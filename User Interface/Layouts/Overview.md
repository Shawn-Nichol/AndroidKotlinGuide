# What is a layout
A layout defines teh structure for a user interface in your app, such as in an activity. All elements in the layout are built using a heirarchy of `View` and `ViewGroup` objects. A `View` usually draws something the user can see and interact with. Whereas a `ViewGroup` is an invisible container that defines the layout stucture for `View` and other `ViewGroup` objects. 

The `View objects are usually called "widgets" and can be one of many subclasses, such as `Button` or `TextView`. The `ViewGroup objects are usually called "layouts" and can be one of many types that povide a different layout structure, such as `LinearLayout` or `ConstraintLayout`.

You can declare a layout in two ways. 
1) Declare UI elements in XML
Android provides a straightforward XML vocabulary that corresponds to the View classes and subclasses, such as those for widgets and layouts. You can aslo use Android Studio `Layout Editor` to build your XML layout.

2) Instantiate layout elements at runtime.
Your app can create View and ViewGroup objects progammatically. 

Declaing your UI in XML allows you to separate the presentation of your app from the code that controls its behavior. using XML files also makes it easy to provide different layouts for different screen sizes and orientations. 

The Android framework gives you teh flexibility to use either or both of these methods to build your app's UI. For example, you can declare your app's default layouts in XMl, and then modify the layout at runtime. 

## Write XML
Using Android's XML vocabulary, you can quickly design UI layouts and the screen elements they contain, in the same way you create web pages in HTML witha series of nested elements. 

Each layout file must contain exactly one root elemtn, which must be a View or ViewGroup object. Once you've defined the root element, you can add additional layot objects or widges as child elements to gradually build a View hierarchy that defines your layout. For example, here's an XML layout that uses a vertical `Linearlayout` to hold a `TextView` and a `Button`.

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical" >
    <TextView android:id="@+id/text"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="Hello, I am a TextView" />
    <Button android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello, I am a Button" />
</LinearLayout>
```

After you've declare your layout in XMl, save the file with the .xml extension, in your andoird progjects `res/layout/` directory, so it will properly compile.

## Load the XML Resource
When you compile your app, each XML layout file is compiled into a `View` resource. You should load the layout resource from your app code, in your Activity.onCreate() callback implementation. Do so by calling setContentView9), passing it the reference to your layout resource in the form of R.layout.layout.layoutfile_name. 

```
fun onCreate(...) {
  super.onCreate(savedInstanceState)
  setContentView(R.layout.main_layout)
}
```

## Attributes
Every View and ViewGroup object supports their own variety of XML attributes. Some attributes are specific to a View object(for example, TextView supports the `textSize` attribute), but these attributes are also inhertied by any View objects that may extend this class (like the id attrbiute). And, other attributes are considered "layout paramaeters", which are attributes that descirbe certain layout orientations of the View object, as defined by that object's parent ViewGroup object.

## ID
Any View object may have an integer ID associated with it, to uniquley identify the View within the tree. When the appis compiled, this ID is referenced as inteeger, but the ID is typically assigned in teh layout XML file as a string, in teh id attribute. This is an XML attribute common to all View objects(defined by the `View` class) and you will use it very often. The sytnax for an ID, inside an XML tag is 
```
android:id="@+id/my_button"
```

The at-symbol at thte beginning of the string indicates that the XML parser should parse and expand the rest of the ID string and identify it as an ID resource. The plus sybmol means that this is a new resource name that must be created and added to our resources (in the `R.java` file). There are a number of other ID resources that are offered by the Android framework. When referencing an Android resource ID, you do not need the plus symbol, but must add the android package namsepace
```
android:id="@id/empty"
```
With the andorid package namspace in place, we're now referencing an ID from the `android.R` resources class, rather than the local resources class. 

In order to create views and reference them form the app, a common patter is to 

1. Define the view/widget int eh layout file and assign it an unique ID. 
```
<Button android:id="@+id/my_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/my_button_text"/>
```

2. Then createa na instance of the view object and cpature it from the layout. 
```
val myButton: Button = findViewById(R.id.my_button)
```

Definning IDs for view objects is improtatn when creating a `RealtiveLayout`. In a realtive layout, sibling views can define their layout realtive to another sibling view, which is referenced by the unique ID. 

An Id need not be unique throughout the entire tree, but it should be unique within the part of the tree youa researching (wich may oftten be the eiter tree, so it's best to be completely unique when possible). 

Note: ViewBinding replaces findViewById

## Layout Paramaters
XML layout attributes named `layout_something` define layout parameters for the View that are appropriate for the ViewGroup in which it resides. 

Every ViewGroup class implements a nested class that extends `ViewGroup.LayoutParams`. This subclass contains property types that define the size and position for each child view, as appropriate for the view group. 

Note that every LayoutParams subclass has its own syntx for setting vlues. Each child element must define LayoutParamas that are appropriate for its parent, through it may also define different layoutParams for its own children

All view groups include a width and height (layot_width and layout_height), and each view is required to define them. layoutParams may also include optional margins and borders. 

you can specify width and height with exact measurments, though you probaly won't want to do this often. More often,you will use one of these constants to set the width of height.

- wrap_content tells your view to size itself to the dimension required by its content. 

- match_parent tells your view to become as big as its parent view group will allow. 

In general specifying a layout width and height using absolute units such as pixels in not recommend. Instead, using relative measruements such as density-indpement pixel units(dp), wrap_content, or match_parent, is a better approach, becuase it helps ensure that your app will dsiplay properly across a variety of device screen sizes. The accepted meauremnt types are define in the aviable Resource do
