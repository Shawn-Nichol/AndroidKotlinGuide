The canvas class holds the draw calss. To draw something you need 4 basic componenets:
1) Bitmap: to hold the pixels where the canvas will be drawn
2) Canvas: to run the drawing commands.
3) Drawing commands: to indicate to the canvas what to draw. 
4) Paint: descibe the colors and styles of the drawing. 

The coordinate system of the Android canvas starts in the top left corner where [0,0] represents that point.  
- Y axis is postive downwards
- X axis is Postive towards the right. 

All elements drawn on the canvas are placed relative to the [0,0] point.
When working with the Canvas you are working with px and not dp. This will ensure that your drawing looks consistent across multiple devices with different pixel desnsities. 

Canvas draw commans will draw over previously drawn items. The last draw command will be the topmost item drawn onto your canvassssss. It is up to you to ensure that your items are laid out correctly (alternatively, you might want to use some of the built-in layout mechansisms for this such as a LinearLayout. 

In order to create a canvas you will need to create a class that extends from view. This will then allow you to override the onDraw method, which has a cnvas as a parameter. 
You can then include this view inside your layout xXML and theis will then automatically invoke the onDraw method of the Custom View. 

Programical access. 
create a bitmap and then a canvas object
```
val bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888)
val canvas = Canvas(bitmap)
```
Its worth noting at this point that any Canvas created programmactically without using a view, will be software rendered and not hardware rendered. This can affect the appeara
