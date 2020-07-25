# Clipping

Clipping is a way to define regions of an immage, canvas or bitmap that are selectively drawn or not drawn onto the screen. One purpose of clipping is to reduce overdraw.
Overdraw is when a pixel on the screen is drawn more than once to display the final image. When you reduce overdraw, you minimize the number of times a pixel or region of
the display is darawn, in order to maximize drawing performance. You can also use clipping to create interesting effect in the UI design and animations.

The clipping region is commonly a rectangle, but it can be any shape or combination fo shapes, even text. You can also specify whether you want the region inside th e clipping region included or excluded. For example you could createa circular clipping region and only display what's outside the circle. 

The alogrithm used to draw the rectagnles
1) Translate Canvas to where you want the rectangle to be drawn. This is instead of calcualting where the next rectangle and all the other shpaes need to be drawn,
you move the canvas origin, that is, its coordinate systme.
2) Draw the rectagnle at the new origin of the canvas. That is you draw the shapes at the same lcoation in the tranlated coordinate system. THis is a lot simpler and slighlty
more efficient. 
3) You restore the Canvas to its orignal orgin.

For each rectangle 
1) save the current state of teh canvase so you can reset to the intial state.
2) Translate the origin of the canvas to the lcoation where you want to draw.
3) Apply clipping shapes and paths
4) Draw the rectangle or text
5) Restore the state of the canvas

Task Implment clipping methods
1) save the current state of the canvas: 'canvas.save()'
The activity context maintains a stack of drawing states. Drawing states consist of the current transformation matrix and the current clipping region. You can save
the current state, perform actions that change the drawing state such as translating or rotating the canvas and then restore the saved drawing state. Note this is like
the stach command in git

When your drawing includes transformations, chainging and undoing transformations by reversing them in is error-prone. For example, if you translate, strctch, and then rotate, it gets complex quickly. Instead save the state of the canvas, apply your transformation, draw and then restore the previos state.
2) Translate the origin of the canvas to the row/ column coordinate canvas. 
It is much simpler to move teh origin of the canvas and draw the same thing in a new cordianate system than to move all the elements to draw. You can use the same techinque for rotating elements
3) Apply transformations to the path, if any
4) Apply clipping. 
5) Draw shapes
6) Restore the previous canvas state.
