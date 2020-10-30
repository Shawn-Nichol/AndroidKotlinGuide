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

In general specifying a layout width and height using absolute units such as pixels in not recommend. Instead, using relative measruements such as density-indpement pixel units(dp), wrap_content, or match_parent, is a better approach, becuase it helps ensure that your app will dsiplay properly across a variety of device screen sizes. The accepted meauremnt types are define in the aviable Resource documentations

https://developer.android.com/guide/topics/resources/available-resources#dimension


## Layout Position
The geometry of a view is that of a rectangle. A view has a location, expressed as a pair of left and top coordinates, an d tow dimensions, expressed as a width and height. The unit for location and dimesnions is the pixel. 

it is possible to retrieve the location ov a view by invoking the mthods getLeft() and getTop(). Teh former returns the left, or X, correcinate of the rectangle repsresnting the view. The latter returns the top, or Y, coordinate of the rectagle representing the view. these methods both return the location ov the view relative to its parent. For instance, when `getLeft()` returns 20, that mean sthe view is located 20 pixels to the righ to fthe left edge of its direct parent. 

In addtion, several convenince methods are offered to avoid unnececsssary computations, namely `getRight()` and `getBottom()`. These methods return the corrdinates of the right and bottom edges of the rectangle representing the view. For instance calling `getRight()` is similar to the follwoing computation `getLeft() + getWidth()`.


## Size, Padding and Margins
The size of a view is expressed with a width and height. View actually possess two pairs of widht and height values. 

The first pair is known as the measured width and measured height. Thse dimensinos define how big a view wants to be within its paretn. The mearured dimensions can be obtained by calling `getMeasuredWidth()` and `getMeasuredHeight()`. 

The second pair is simply known as width and height, or sometimes drawing width and drawing height. These dimensions define the actual size of the view on screen, at drawing time and after layoutl These values may, but do not have to, be different from the measured width and height. The width and height can be obtained by calling `getWidth()` and `getHeight()`.

To measure its dimensions, a view takes inot account its padding. The padding is expressed in pixels for left, top, right and bottom parts of the view. Padding can be used to offset the vie by a specific number of pixels. For instance, a left padding of 2 will push the view's content by 2 pixels to the right of the left edge. Padding can be set using the `setPadding(int, int, int, int)` method and queried by calling `getPaddingLeft()`, `getPaddingTop()`, `getPaddingRight()` and `getPadddingBottom()`.

Even though a veiw can define a padding, it does not provide any support for margins. however, view groups provide such as uspport. Refer to `ViewGroup` and `ViewGroup.MarginlayoutParams` for further information.

## Common Layouts
Each subclass of the `ViewGroup` class provides a unique way to display the view you nest within it. 

LinearLayout: Alayout that organizes its children into a single horizontal or vertical row. It creates a scrollbar if the length of the window exceeds the length of the screen. 

Relative Layout
Enables you to specify the location of child objects relative to each other(child A to left of chilc B or to the parent aligend to the top of the parent. 

Web View
Displays web pages. 

## Building Layouts with an adapter. 
When the content for your layout is dynamic or not pre-determined, you can use a layout that subclasses `AdapterView` to populate the layout with views at runtime. A subclass of the `AdapterView` class uses an `Adapter` to bind data to its layout. The `Adapter` behaves as a middleman between the data source and the `AdapterView` layout the `Adapter` retreives the data (from a srouce such as an array or a database query) and converts each entry into a view that cna be added into the `AdapterView layout`. 

ListView:
Displays a scrolling single column list

GridView:
Displays a scolling grid of columns and rows. 

## Filling an adapter view witth data
You can populate an `AdapterView` such as `ListView` or `GridView` by binding the `AdapterView` instance to an Adapter, which retrieves data from an external source and creates a `View` that repsresnets each data entry. 

Android provides serveral subclasses of `Adapter` that are useful for retrieving different kinds of data and buildng views for tan `AdapterView`. The two most common adaters are

`ArrayAdapter`
Use this adapter when your data source is an array. By default, `ArrayAdapter` creates a view for each array item by calling `toString()` on each item and placing the contents in a `TextView`

For example, if you have an array of string you want to display in a `ListView`, initialize a new `ArrayAdapter` using a constructor to specify the layout for each string adn the string array
```
val adapter = ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, myStringArray)
```
The arugmetns for this constructor are 
  - Your app `Context`
  - The layout that contains a `TextView` for each string in the array
  - The string array
  Then simply call `setAdapter()` on your `ListView`
```
val listView: ListView = findViewById(R.id.listview)
listView.adapter = adapter
```
To customize the appearance of each item you can override the `toString()` method for the objects in your array. Or, to create a veiw for each item that's someting other than a `TextView` for example, if you want an `ImageView` for each array item), extend the `ArrarAdapter` class and override `getView()` to return the type of vie wyou want for each item.

`SimpleCursorAdapter`
Use this adapter when your data comes from a `Cursor`. When using `SimpleCursorAdapter`, you must specify a layout to use for each row in the `Cursor` and which columns in the `Cursor` should be inserted into which views of the layout. For example, if you want to create a list of people's names an dphone numbers, you can perform a query that returns a `Cursor` containg a row for each person and columsn for the name and numbers. You then create a string array specifying which columns from the `Cursor` you want in the layout for each result and an integer array specifying the corresponding views that each column should be placed. 
```
val fromColumns = arrayOf(ContactsContract.Data.DISPLAY_NAME,
                          ContactsContract.CommonDataKinds.Phone.NUMBER)
val toViews = intArrayOf(R.id.display_name, R.id.phone_number)
```

When you instantiate the `SimpleCursorAdapter`, pass the layout to use for each result, the `Cursor` containg the results, and these two arrays. 

```
val adapter = SimpleCursorAdapter(this,
        R.layout.person_name_and_number, cursor, fromColumns, toViews, 0)
val listView = getListView()
listView.adapter = adapter
```
The `SimpleCursorAdapter` then creates a view for each row in the `Cursor` using the provided layout by inserting each `fromColumns` item into the corresponding `toViews` view.

If, during the coruse of your app's life, you change the underlying data that is read by your adapter, you should call `notifyDataSetChanged()`. This will notify the attached view that the data has been changed and it should refresh itself. 

## Handling click events

You cna respond to click events on each item in an `AdapterView` by implementing the `AdapterView.OnItemClickListener` interface. 
```
listView.onItemclickListener = AdapterView.OnItemClickListener { parent, view, position, id -> 
  // Do something
}
```
