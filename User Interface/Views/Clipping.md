# Clipping

Clipping is a way to define regions of an immage, canvas or bitmap that are selectively drawn or not drawn onto the screen. One purpose of clipping is to reduce overdraw.
Overdraw is when a pixel on the screen is drawn more than once to display the final image. When you reduce overdraw, you minimize the number of times a pixel or region of
the display is darawn, in order to maximize drawing performance. You can also use clipping to create interesting effect in the UI design and animations.

The clipping region is commonly a rectangle, but it can be any shape or combination fo shapes, even text. You can also specify whether you want the region inside the clipping region included or excluded. For example you could createa circular clipping region and only display what's outside the circle. 

Clipping can be any combination of shapes and paths. 

Clipping steps 
1) set type of clilping you want and the location
2) Depending on the type of clipping selected the canvas will draw in side the boundaries or outside the boundaries. 
