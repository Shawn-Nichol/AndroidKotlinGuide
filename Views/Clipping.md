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


## Quick Reject
The quickReject() canvas method allows you to check whether a specified rectangle or patht would lie completely ouside the curretnly visible regions, after all transformations have been applied. 

The quickReject() method is incredibly useful when you are constructing more complex drawings and need to do so as fast as possible. With quickReject(), you can decide efficiently which objects you do not have to draw at all, and there is no need to write your own intersection logic. 
  - The quickReject() method returns true if the rectangel or path would not be visible at all on the screen. For partial overlaps, you stil have to do your own checking.
  - The EdgeType is ether AA (antialiased: treat edges by rounding-out, becuase they may be antialiased) or BW(black-white: treat edges by just rounding to the nearest pixel boundary) for just rounding to the nearest pixel.


## Summary
- The context of an activity maintains a state that preserves transormations and clipping regions for the canvas.
- Use canvas.save() and canvas.restore to draw and return the original state of your canvas
- To draw multiple shapes on a canvas, you can either calculate their location, or you can move (Translate) the origin of your drawing surface. The latter can make it easier to create utility methods for repeated draw sequences. 
- Clipping regions can be any shape, combinations of shapes or path.
- You can add, subtract, and intersect clipping regions to get exactly the region you need.
- You can apply transformations to text by trasnformign the canvas
- the quickReject() canvas method allows you to check whether a specified rectagnel or path would lie completely outside the currently visible regions.