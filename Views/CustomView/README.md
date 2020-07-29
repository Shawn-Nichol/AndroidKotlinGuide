# How to create a Custom View
1) Create a class with a primary constructor(there are 4 constructors to choose) that extends View. 
2) Declare a paint Property, and provide some colors. 
3) Override onDraw(), onDraw delivers a Canvas to which you can implement anything you want in 2D. 
   - Use 'something.reset()' to remove any old path before drawing a new path. 
   - To optimize performance, assign any required values of drawing and painting before using them in onDraw(), such as in the constructor or init() helper method
4) Add listeners to custom view interactions
5) Create a Attrs.xml file in values folder, you can set the custom views attribute in here
6) Add the custom view to an XML lalyout file with attributes to define its  apperaance, as you would witih other UI elements. 
7) Override onMeasre() (is optional): is critical pice of the rendering contract between your component and its container. Overriding onMeasure() will efficiently and accuratley report the measurements of its contained parts. Must call setMeasuredDimensions method with measured width and height once they have been calcualted, failing todo so will result in exception at run time. 



```
class MyCircle(context: Context, attrs AttributesSet) : View(context, attrs) {
  // Paint object for coloring and styling
  private paint = Paint(Paint.ANTI_ALIAS_FLAG)
  
  // Shape details
  private var circleColor = Color.GREEN
  private var boarderColor = Color.BLACK
  private boarderWidth = 4.0f
  private var size = 320
  
  override fun onDraw(canvas: Canvas) {
    super.onDraw(canvas)
    
    paint.color = circleColor
    paint.style = Pain.Style.FILL
    val radius = size / 2f
    
    canvas.drawCircle(size / 2f, size / 2f, radius, paint)
    
    paint.color = boarderColor 
    paint.style = Paint.Style.STROKE
    paint.strokeWidth = boarderWidth
    
    canvas.drawCircle(size/2f, size /2f, radius, paint)
  }
  
  // onMeasured isn't used in this example but it would like something like this

  override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) { 
    super.onMeasure(widthMeasureSpec, heightMeasureSpec)
    size = Math.min(measuredWidth, measuredHeight)
    setMeasuredDimension(size, size)
  }
}
```

To draw onto a canvas you will need the following

- Bitmap: to hold the pixels where the canvas will be drawn
- Canvas: to run the drawing commands.
- Drawing commands: to indicate to the canvas what to draw. 
- Paint: descibe the colors and styles of the drawing. 


When creating a custom view you should follow these steps
1) Save the current state of the canvas 'canvas.save()'
The Activity context maintains a stack of drawing states. Drawing states consists of the current transformation matrix and the current clipping regioin. You can save teh crrent state, perform actions that change the drawing state such as translating or rotating the canvas and then restore the saved drawing state.
2) Translate: move the orgin of the canvas and draw something in a new cordianate
3) Transformations to the path
4) Apply clipping
5) Draw shapes
6) Restore the previous canvas state.
