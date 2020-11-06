# Creating a View class
A well-desinged custom view is much like any other ewll-desinged class. It encapsulates a specific set of functionality with an easy to use interface, it uses CPU and memory efficiently, and so on. In addition to being a well-desinged class, though a custom view should

- Conform to Android standards
- Provide custom styleable attributes that work with Android XML layouts
- Send accessibility events.
- Be compatible with multiple Android platforms. 

The Android framework provides a set of base classes and XML tags to help you createa  view that meets all of these requirements. THis lesson dicsusses how to use the Android famework to create the core functionality of a view class. 

Beyond this lesson, you can find additional, realted information in custom components

## Subclass A View
All of the view classes defined in the Android framework wextend `View`. Your custom view can also extend `View` directly,  or you can save time by extending one of the existing view subclassses, such as `Button`.

To allow Android Studio to interact with your view, at a minimum you must provide a constructor that takes a `Context` and an `AttributeSet` object as parameters. This constructor allows the layout editor to create and edit an instance of your view. 

```
class PieChar(context: Context, attrs: AttributeSet) : View(context, attrs)
```

## Define Custom Attrbiutes
To add a built-in `View` to your user interface, you specify itin an XML element and control its appearance and hehavior with element attributes. Wll-written custom views can also be added and styled via XML. To enable this behavior in your custom view you must. 

- Define custom attributes for your view in a `<declare-styleable>` resource element. 

- Specify values for the attributes in your XML layout. 

- Retrieve attribute values at runtime

- Apply the retrieved attribute values to your view. 

This section discusses how to define custom attributes and specify their values. The next section deals with retrieving and applying the values at runtime. 

To Define custom attributes, add `<declare_styleable>` resourece to your project. It's customary to put these resource in res/values/attrs.xml file

```
<resources>
   <declare-styleable name="PieChart">
       <attr name="showText" format="boolean" />
       <attr name="labelPosition" format="enum">
           <enum name="left" value="0"/>
           <enum name="right" value="1"/>
       </attr>
   </declare-styleable>
</resources>
```

This code delcares two custom attributes, `showText` and `labelPosition`, that belong to a styleable entity named `PieChart`. The name of the styleable entity is, by convention, the same name as  the name of the class that defines the custom view. although it's not strictly necessary to follow this convention, many popular code editors depend on this naming convention to provide statement completion. 

Once you define the custom attributes, you can use them in layoutXML files just like built-in attributes. THe only difference is that you custom attributes belon to a different namespace. Instead of belonging to he `http://schemas.android.com/apk/res/android` namespace, they belong to `http://schemas.android.com/apk/res/[your package name]`
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:custom="http://schemas.android.com/apk/res/com.example.customviews">
 <com.example.customviews.charting.PieChart
     custom:showText="true"
     custom:labelPosition="left" />
</LinearLayout>
```

In order to avoid having to repeat the long namespace URI, the sample uses an `xmlns` directive. This directive assigns the alis custom to the namespace. You can choose any alias you want for your namespace. 

Notice the name of the XML tag that adds the custom view to the layout. It is the fully qualified name of the custom view class. If your view class is an inner class, you must further qualify it with the name of the view's outer class. For instance, the PieChart class has an inner class called `PieView`. To use the custom attributes form this class, you would use the tag `com.exmple.customviews.charting.PieChart$PieView`.


## Apply Custom attributes 
When a view is created form an XML layout, all of the attributes in the XML tag are read from the resource bundle and passed into the view's constructor as an `AttributeSet. Although it's possible to read values form the `AttributeSet` directly, doing so has some disadvantages:
- Reosurce reference within attribute values are not resolved.
- Styles are not applied. 

Instead, pass the `AttributeSet` to `obtainStyledAttrbiutes()`. This method passes back a `TypedArray` array of values that have already been dereferenced and styled. 

The android resource compiler does a lot of work for you to make calling `obtainStyledAttributes()` easier. For each `<declared-styleable>` resource in the res directory, the generated R.java defines both an array of attribute ids and a set of constants that define the index for each attribute in the arry. you use the predefined constants to read the attributes from the `TypedArray`. Here's how the PieChar class reads its attributes: 

```
init {
    context.theme.obtainStyledAttributes(
            attrs,
            R.styleable.PieChart,
            0, 0).apply {

        try {
            mShowText = getBoolean(R.styleable.PieChart_showText, false)
            textPos = getInteger(R.styleable.PieChart_labelPosition, 0)
        } finally {
            recycle()
        }
    }
}
```

Note: that `TypedArray` objects are a shared resource and must be recycled after use. 

## Add Properties and Events
Attributes are a powerful way of controlling the behavior and appearance of views, but they can only be read when the view is intialized. To provide dynamic behavior, expose a property getter and setter pair for each custom attribute. The following snippet shows how  `PieChart` exposes a property called `showText`.

```
fun isShowText(): Boolean {
    return mShowText
}

fun setShowText(showText: Boolean) {
    mShowText = showText
    invalidate()
    requestLayout()
}
```

Notice that `setShowText` calls `invalidate()` and `requestLayout()`. These calls are crucial to ensure that the views hehaves reliably. You have to invalidate the view after any change to its properties that might change its appearance, so that the system knows that it needs to be redrawn. Likewise, you need to request a  new layout if a property changes that might affect the size or shape of the view. Forgetting thesee methods calls can cause hard-to-find bugs. 

Custom views should also support event listeners to communicate important events. For instance `PieChart` exposes a custom event called `OnCurrentItemChanged` to notify listeners that the user has rotated the pie chart to focus on a new pie slice. 

It's easy to forget to expose properties and events, especially when you're the ony user of the custom view. Taking some time to carefully define your view's interface reduces future maintenance costs. A good rule to follow is to always expose any property that affects the visible appearance of behavior of your custom view. 

## Design for Accessibility
Your custom view hsould support the widest range of users. This includes users with disabilities that prevent them from seeing or using a touchscreen. To support users with disabilities, you should 
- Label your input fileds using the Android:contentDescription attribute
- Send accessibility events by calling `sendAccessibilityEvent()` when appropriate. 
- Support alternate controllers, such as D-pad and trackball. 

