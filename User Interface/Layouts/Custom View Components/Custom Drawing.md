# Implementing Custom drawing
The most important part of a custom view is its appearance. Custom drawing can be easy or complex according to your application's needs. THis lesson covers some of the most common operations. 

Beyon this leeson, you can find additonal realted informationat Canvas and drawables. 

## Override onDraw()
The most improtatn step in drawing a custom view is to override the `onDraw()` method. The parameter to `onDraw()` is a `Canvas` object that the view cna use to ddraw itself. The `Canvas` class defines methods for ddrawing text, lines, bitmaps, and many other graphics primitievs. YOu can use these methods in `onDraw()` to create your custom user interface

Before you can call any drawing methods, though it's necessary to create a `Paint` object. The next section discusses `Paint` in more detail

## Create Drawing Objects
The android.graphics famework divides drawing into two areas
- What to draw, hadnled by `Canvas`
- How to draw, handled by `Paint`

For instance, `Canvas` provides a method to draw a line, while `Paint` provides methods to define that line's color. `Canvas has a method to draw a rectangle, while `Paint` defines whether to fill that rectangle with a color or leave it empty. Simply put, `Canvas` defines shpaes that you can draw on tot he screen, while `Piant` defines the color, style, font and so forth of each shape you draw. 

So, before you draw anything, you need to create one or more `Paint`` objects. The `PieChart` example does this in a emthod called init, which is calle dfomr the constructor from java, but we can initialise inline in kotlin. 

```
private val textPaint = Paint(ANTI_ALIAS_FLAG).apply {
    color = textColor
    if (textHeight == 0f) {
        textHeight = textSize
    } else {
        textSize = textHeight
    }
}

private val piePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
    style = Paint.Style.FILL
    textSize = textHeight
}

private val shadowPaint = Paint(0).apply {
    color = 0x101010
    maskFilter = BlurMaskFilter(8f, BlurMaskFilter.Blur.NORMAL)
}
```

Creating objects ahead of time is improtantt optimization. Views are redrawn very frequently, and many drawing objects require expensive initialization. Creating drawing objects within your `onDraw() method significantly reduces perfomrance and can make your UI appear sluggish. 

## Handle Layout Events. 
In Order to peroply draw your custom view, you need to know what size it is. COmples custom views often need to perfomr multiple layout calcualtions depending on the size and shape of their area on screen. you should never make assumptions about the size of your view on the screen. Even if only one app uses your view, that app needs to handle differen screen sizes, multple screen densities, and various aspect ratios in both protrait and landscape mode. 

Although `View has many mehtods for handling measurement, most of them do not need to be overridden. if your view doesn't need special control over its size, you only need to override on mehtod onSizeChanged(). 

onSizeChanged() is called when your view is first assigend a size, and again if the size of your view changes for any reason. Calculate positions, dimensions, and any other values related to your view's size in `onSizeChanged(), instaead of recalculating them every time you draw. In the `PieChart` example, `onSizedChagned()` is where the PieChart view calcualtes the boundign rectangles of the pie chart and the relative postion of the text label and other visual elements. 

When your ivew is assigne da size, the layout manager assumes that sthe size includes all of the view's padding. You must handlethe padding values when you calculate your view's size. Here's a snippet form `PieChar.onSizeCHanged(0 that shows how to do this. 

```
// Account for padding
var xpad = (paddingLeft + paddingRight).toFloat()
val ypad = (paddingTop + paddingBottom).toFloat()

// Account for the label
if (showText) xpad += textWidth

val ww = w.toFloat() - xpad
val hh = h.toFloat() - ypad

// Figure out how big we can make the pie.
val diameter = Math.min(ww, hh)
```

If you need finere control over your view's layout parameters, implement `onMeasure()`. This methods parameters are `View.MeasureSpec` values that tell you how big your view's parent wants your view to be, and whether that size is hard maximum or jsuta  ssuggestion. As an optimization, these values are stored as packed integers, and you use the static methos of `View.MeasureSpec` to unpack the information stored in each integer. 

Here's an  exmape implementation of `onMeasure()`.
```
override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    // Try for a width based on our minimum
    val minw: Int = paddingLeft + paddingRight + suggestedMinimumWidth
    val w: Int = View.resolveSizeAndState(minw, widthMeasureSpec, 1)

    // Whatever the width ends up being, ask for a height that would let the pie
    // get as big as it can
    val minh: Int = View.MeasureSpec.getSize(w) - textWidth.toInt() + paddingBottom + paddingTop
    val h: Int = View.resolveSizeAndState(
            View.MeasureSpec.getSize(w) - textWidth.toInt(),
            heightMeasureSpec,
            0
    )

    setMeasuredDimension(w, h)
}
```

There are three improtant things to note. 

- The clculations take into account the view's padding. As mentioned earlier, this the view's responsibility. 

- The helper method `resolveSizeAndState()` is used to create the final width and height values. This helper returns an appropriate `View.MeasureSpec` value by comparing the view's desired size to the spec passed into `onMeasure()`

## Draw
Once you have your object creation and measuring code defined,you can impelment `onDraw()`. Every view implements `onDraw() differently, but there are some common operations that most views share: 
- Draw text using `drawText()`. Specify the typeface by calling `setTeypeface()`, and the text color by calling `setColor()`. 
- Draw primitive shapes using `drawRect()`, `drawOval()`, and `drawArc()`. Change whether the shpaes are filled, outlined, or both by calling `setStyle()`. 
- Draw more complex shapes using the `Path` class. Define a shape by adding lines and curves to a `Path` object, then draw the shape using `drawPath()` class. Just as with primitive shapes, paths can be outlined, filled or both, depending on the `setStyle(). 
- Define gradient fills by creating `LinearGradient` objects. Call `setShader()` to use your `LinearGradient` on the filled shapes. 
- Draw bitmaps using `drawBitmap()`. 
```
override fun onDraw(canvas: Canvas) {
    super.onDraw(canvas)

    canvas.apply {
        // Draw the shadow
        drawOval(shadowBounds, shadowPaint)

        // Draw the label text
        drawText(data[mCurrentItem].mLabel, textX, textY, textPaint)

        // Draw the pie slices
        data.forEach {
            piePaint.shader = it.mShader
            drawArc(bounds,
                    360 - it.endAngle,
                    it.endAngle - it.startAngle,
                    true, piePaint)
        }

        // Draw the pointer
        drawLine(textX, pointerY, pointerX, pointerY, textPaint)
        drawCircle(pointerX, pointerY, pointerSize, mTextPaint)
    }
}

```
