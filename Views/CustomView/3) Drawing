# Drawing

The onDraw() method has a canvas parameter, the view can use the canvas object to draw itself. The onDraw() method requires a class that extends View. 

## How to Instantiate a drawable
There are two wasy to define and instantiate a drawable besides using the class constructors. 
- Inflate an image resouce(a bitmap file)
- Infalte an XML resource that defines the drawable properties. 

## When is onDrawCalled
The onDraw() is called whenever android thinks that your view should be redrawn. This can be the case when your view is animated in which case onDraw is called every frame in the animation. It also called when the layout changes and your view is re-poistioned on the screen. 

## My Notes
- Anything that takes a lot of time should either be done during initialisation or in a separate thread. Allocatin gobjects wit new can be a performance drag, especially if done repeatedly, for this reason avoid allocating obejcts within onDraw().

- super.onDraw() can be omitted, view.onDraw has an empty implementation and calling it won't make any difference to how the view appers. 



Example
```
class MyDrawing(context: Context, attrs: AttributeSet) : View(context, attrs) {

  override fun onDraw(canvas: Canvas) {
  }

}
```
