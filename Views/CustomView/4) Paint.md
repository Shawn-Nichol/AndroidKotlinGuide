 # Paint
 
 ## isAntiAlias
 Defines whether to apply edge smoothing. Setting isAntiAlias to true, smoothe out the edges of what is drawn without affecting the shape.
 
 ## isDither
 When true, affects how colors with hiegher-precision than the device are down-sampled
 
 
 ## style
 Sets the type of painting to be done to a stroke, which is essentially a ling. Paint.Style specifies if the primitive being drawn is filled, stroked or both. The default is to fill the object o which the paint is applied
 
 To fill and image and draw a boarder, write fill command draw the pain stroke and draw.
 ```
 paint.style = fill
 paint.color = Color.Yellow
 canvas.draw(w, h, 0, 0, paint)
 ```
 
 ## storkeJoin
 Specifies how lines and curve segments join on a stroke path. The default is MITER
 
 ## strokeCap
 sets the shape of the end of the line to be a cap. Paint.Cap specifies how the begining and ending of stroked lines and paths. The default is BUTT
 
 ## strokeWidth
 Specifies the width of the strke in pixels. Thed default is hairline width, which is really thing. 
 
## Path
The path class encpsulates compound (multiple contour) geometric paths consisting of straight line segments, quadratice curves, and cubic curves. It can be drawn with canvas. drawPath(path, paint), either filled or stroked or it can be used for clipping
