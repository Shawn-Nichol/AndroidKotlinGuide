# How to create a CustomView



# Creating a View class
Create a class with Context and AttributeSet arguments. Extned the View or a subclass of view and pass the Context and AttributeSet from the constructor. 

```
class MyCustomView(context: Context, attrs: AttributeSet) : View(context, attrs) {

}

```

## Override onSizeChanged
Called on intial load to check the size of view, then called each time the view size changes. 
@ w: current width of the view.
@ h: current height of the view.
@ oldW: old width of the view.
@ oldH: old height of the view. 

```
 override fun onSizeChanged(w: Int, h: Int, oldw: Int, oldh: Int) {
     radius = (min(w, h) / 2 * 0.8).toFloat()
     centerX = width / 2.0f
     centerY = height / 2.0f
 }
```

## onDraw
This is were you set the parameters to draw the view. 

@ canvas: is the space the View can be drawn in. 

```
override fun onDraw(canvas: Canvas) {
   ... 
}
```

## Define your paint
Paint class holds the style color infomration about how to draw the class. 
```
    private val paintText = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.CYAN
        style = Paint.Style.FILL_AND_STROKE
        textSize = 200.0f
        strokeWidth = 10.0f
        textAlign = Paint.Align.CENTER
    }
```

## Define Custom Attrbiutes
Create an attrs file in `Value` directory, define the custom attributes for your view in a `<declare-styleable>` resource element. '<declared-stylable>` requires the same name as the CustomView class. 
```
<resources>
    <declare-styleable name="MyCustomView">
        <attr name="color1" format="color"/>
        <attr name="color2" format="color"/>
        <attr name="color3" format="color"/>
    </declare-styleable>
</resources>
```

Specify values for the attributes in your XML layout. 
```
<com.example.MyCustomView
   android:id="@+id/xmen"
   android:layout_width="200dp"
   android:layout_height="200dp"
   app:color1="#FFFF00"
   app:color2="#FF0000"
   app:color3="#000080"/>
```
Note: custom attributes belong to a different namespace. Instead of belonging to he `http://schemas.android.com/apk/res/android` namespace, they belong to `http://schemas.android.com/apk/res/[your package name]`


- Apply Custom attributes 
In the CustomView class create `init` block and use withStyledAttributes to retrive the resources
```
private var color1 = 0
private var color2 = 0
private var color3 = 0

init {
   context.withStyleAttributes(attrs, R.styleable.MyCustomView) {
      color1 = getColor(R.styleable.MyCustomColor_color1, 0)
      color2 = getColor(R.styleable.MyCustomColor_color2, 0)
      color3 = getColor(R.styleable.MyCustomColor_color3, 0)
   }
}
```


## Hanlde clicks
In the `init` block set the isCLickable to true
```
init {
   isClickable = true
   ...
}
```

## performClick
Call this view's onClickListener, if it is defined. Performs all normal action associateed with clicking reporting accessibility event
@boolean: True ther ewas an assigned OnClickListener that was called, false other wise. 

```
override fun performClick():Boolean {
   // Must be called first, which enables accessiblity events as well as calls onClickListner()
   if(super.performCLick()) return true
   
   ...
   
   return true. 
}
```
