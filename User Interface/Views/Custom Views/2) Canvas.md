# Canvas 
The Canvas class holds the draw calls. 

## How to Instantiate a Canvas
In order to create a canvas you will need to create a class that extends from view. This will then allow you to override the onDraw method, which has a canvas as a parameter. 
You can then include this view inside your layout XML and theis will then automatically invoke the onDraw method of the Custom View. 

## When is canvas called.
call canvas in onDraw when you are ready to draw view. 


Canvas draw commands will be draw over previously drawn items. The last draw command will be the topmost item drawn onto your canvas. It is up to you to ensure that your items are laid out correctly (alternatively, you might want to use some of the built-in layout mechansisms for this such as a LinearLayout. 

## My Notes
The coordinate system of the Android canvas starts in the top left corner where [0,0] represents that point.  
- Y axis is postive downwards
- X axis is Postive towards the right.

When working with the Canvas you are working with px and not dp. This will ensure that your drawing looks consistent across multiple devices with different pixel desnsities. 




## Examples
Programical access. 
create a bitmap and then a canvas object
```
val bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888)
val canvas = Canvas(bitmap)
```
Its worth noting at this point that any Canvas created programmactically without using a view, will be software rendered and not hardware rendered. This can affect the appeara

