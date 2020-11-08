
## paint

Inner circle 
```
    private val paintCircle = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.GREEN
        style = Paint.Style.FILL

        isDither = true
    }
```

Boarder
```
    private val paintBoarder = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.BLACK
        style = Paint.Style.STROKE
        strokeWidth = borderWidth
        isAntiAlias = true
    }
```
`antiAlias`: Smooths out the edges of what is being drawn, but has no impacton the interior shape
`isdither` No dithering is faster, but higher precision colors are just truncated down. 


## View size
Get the measurment of the View. 
```
    var radius: Float = 0.0f
    var centerX = 0.0f
    var centerY = 0.0f

    override fun onSizeChanged(w: Int, h: Int, oldw: Int, oldh: Int) {
        radius = (min(w, h) / 2 * 0.8).toFloat()
        centerX = width / 2.0f
        centerY = height / 2.0f
    }
```


## onDraw(canvas)
The drawing details
@ canvas: the space the CustomView can be drawn in. 

```
override fun onDraw(canvas: Canvas) {
  drawCircle(canvas)
  drawX(canvas)
}
```

Draw cricle and boarder
```
fun drawCircle(canvas: Canvas) {
    canvas.drawCircle(centerX, centerY, radius, paintCircle)
    canvas.drawCircle(centerX, centerY, radius, paintBoarder)
}
```

Draw dividers in the circle
```
fun drawX(canvas: Canvas) {
  var startAngle = 45.0
  var endAngle = 225.0
  var startX = centerX + (cos(Math.toRadians(startAngle)) * radius).toFloat()
  var startY = centerY + (sin(Math.toRadians(startAngle)) * radius).toFloat()
  var endX = centerX + (cos(Math.toRadians(endAngle)) * radius).toFloat()
  var endY = centerY + (sin(Math.toRadians(endAngle)) * radius).toFloat()

  canvas.drawLine(startX, startY, endX, endY, paintBoarder)


  centerX = width / 2.0f
  centerY = height / 2.0f
  startAngle = 135.0
  endAngle = 315.0
  startX = centerX + (cos(Math.toRadians(startAngle)) * radius).toFloat()
  startY = centerY + (sin(Math.toRadians(startAngle)) * radius).toFloat()
  endX = centerX + (cos(Math.toRadians(endAngle)) * radius).toFloat()
  endY = centerY + (sin(Math.toRadians(endAngle)) * radius).toFloat()
  canvas.drawLine(startX, startY, endX, endY, paintBoarder)
}
```
