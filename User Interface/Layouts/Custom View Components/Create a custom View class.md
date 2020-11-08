# Creating a View class
A CustomView class encapsulates a specific set of functionality with an easy to use interface, it uses CPU and memory efficiently. A custom view should also

- Confrom Android standars
- Provide custom styleable attributes that work with Android XML layouts
- Send accessibility events.
- Be compatible with multiple Android platforms. 

The Android framework provides a set of base classes and XML tags to help you create a view that meets all of these requirements. 

## Subclass A View
All of the view classes defined in the Android framework extend `View`. A CustomView can extend `View` or existing view subclassses, such as `Button`.

A `CustomView` requires a constructor with `Context` and `attributeSet`, this allows the LayoutEditor to create instances of the View. 

```
class MyCustomView(context: Context, attrs: AttributeSet) : View(context, attrs)
```

## Define Custom Attrbiutes
To add a built-in `View` to a user interface, you specify it in an XML element and control its appearance and behavior with element attributes. CustomViews can also be style with XML elements 

- Define custom attributes for your view in a `<declare-styleable>` resource element. 
- Specify values for the attributes in your XML layout. 
- Retrieve attribute values at runtime
- Apply the retrieved attribute values to your view. 

To Define custom attributes, add `<declare_styleable>` resourece to your project. It's customary to put these resource in res/values/attrs.xml file

```
<resources>
   <declare-styleable name="MyCustomView">
       <attr name="showText" format="boolean" />
       <attr name="labelPosition" format="enum">
           <enum name="left" value="0"/>
           <enum name="right" value="1"/>
       </attr>
   </declare-styleable>
</resources>
```

This code delcares two custom attributes, `showText` and `labelPosition`, that belong to a entity named `MyCustomView`. Name the entity the same as the CustomView class name. 

Once custom attributes are defined, you can use them in the XML layout files like built-in attributes. The only difference is that you custom attributes belong to a different namespace. Instead of belonging to he `http://schemas.android.com/apk/res/android` namespace, they belong to `http://schemas.android.com/apk/res/[your package name]`
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:custom="http://schemas.android.com/apk/res/com.example.customviews">
 <com.example.customviews.MyCustomViews
     custom:showText="true"
     custom:labelPosition="left" />
</LinearLayout>
```

In order to avoid having to repeat the long namespace URI, the sample uses an `xmlns` directive. This directive assigns the alis custom to the namespace. You can choose any alias you want for your namespace. 

## Apply Custom attributes 
When a view is created fromm an XML layout, all of the attributes in the XML tag are read from the resource bundle and passed into the view's constructor as an `AttributeSet`. Although it's possible to read values form the `AttributeSet` directly, doing so has some disadvantages:
- Reosurce reference within attribute values are not resolved.
- Styles are not applied. 

Instead, pass the `AttributeSet` to `obtainStyledAttrbiutes()`. This method passes back a `TypedArray` array of values that have already been dereferenced and styled. 

The android resource compiler calls `obtainStyledAttributes()`. For each `<declared-styleable>` resource in the res directory, the generated R.java defines both an array of attribute ids and a set of constants that define the index for each attribute in the array. Use the predefined constants to read the attributes from the `TypedArray`. 

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
Attributes are a powerful way of controlling the behavior and appearance of views, but they can only be read when the view is intialized. To provide dynamic behavior, expose a property getter and setter pair for each custom attribute. 
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

Notice that `setShowText` calls `invalidate()` and `requestLayout()`. These calls are crucial to ensure that the views behaves reliably. You have to invalidate the view after any change to its properties that might change its appearance, so that the system knows that it needs to be redrawn. Likewise, you need to request a  new layout if a property changes that might affect the size or shape of the view. Forgetting these methods calls can cause hard-to-find bugs. 

Custom views should also support event listeners to communicate important events. For instance `MyCustomVIew` exposes a custom event called `OnCurrentItemChanged` to notify listeners that the user has rotated the chart to focus on a new item. 

It's easy to forget to expose properties and events, especially when you're the only user of the custom view. Taking some time to carefully define your view's interface reduces future maintenance costs. A good rule to follow is to always expose any property that affects the visible appearance of behavior of your custom view. 



