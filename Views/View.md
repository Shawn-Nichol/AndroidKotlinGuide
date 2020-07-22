# View
This class representss the basic building block for the UI components. A view occupies a rectangular area on the screen and is responisble for drawing 
and event handling. View is the base class for wdigets which are used to create interactive UI componetns (Buttons, Text, etc) The ViewGroup subclass 
is the base class for layouts, which are invisible containers that hold other Views and define their layout properties. 

## Using View
All of the views in a window are arranged in a single tree. You can add views either from code or by specifying a tree of views in one or more XL layout files. There
are many specialized subclasses of views that act as controls or are capable of displaying text, images or other content. 

Common operations 
- Set Properties: Ex setting the text of a TextView, Properties that are none at build time can be set in the XML layout.
- Set focus: The framework will handle movign focus in response to user input. To force focus to a specific view, call requestFocus()
- Set up listeners: Views all clients to set listeners that will be notified when something interesting happens to the view. 
- Set visibility: You can hide or show a view. 

## IDs
Views may have an integer id associated iwth them. These ids are typically assigned in the layout XML files, and are used to find specific views 
within the view tree.
Define a button in the layout and assign it a unique ID, from teh onCreate method of an Activity find the button. 

## Positioni
The geometry of a view is that of a rectangle. Aview has a location, expressed as a paiar of left and top coordinates, and two dimensions, 
expressed as a width and a height. The unit for location and dimensions is the pixel

It is possible to retrieve the location of a view by invoking the methods getLeft() and getTop(). The former returns the left, or X coordinate 
of the rectangle representing the view. The latter returns the top, or Y coordinate of the rectangle respresenting the view. THeses methods both 
return the location of the view relative to its parent. For instance, when getLeft9) returns 20 that means the view is located 20 pixels to the righ of the left edge of its direc tparent. 

In addtion several convenience methods are offered to avoid unnecessary computations, namely getRight() and getBottom(). These methods return the coordinates 
of the right and bottom edges of the rectangle representing the view. For instance, calling getRight9) is similar ot the following computation: getLeft() + getWidth()

## Size
