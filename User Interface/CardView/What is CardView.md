# What is a CardView
A frameLayout with a rounded corner background and shadow. CardView uses elevation property for shadows and falls back to a custom emulated shadow implementation on older platforms.

Material design CardView
This class upplies Material styles for the card, The widget will display the correct default Material stylse without the use of a style flag. 

Stroke width can be set using th e strokeWidth attribute. Set the stroke color using the stroke color attribute. Without a storke color, the cards  will not render a stroked border, regardless of the stokreWidth value. 

Cards implement checkable, a default way to switch to Android:Checked_State is not provided clients have to call setChecked(boolean. This shows the app: CheckIcon and changees overlay color

Cards also have a custom state meant to be used when a card is draggabel app: Dragged_State. It's used by calling seDragged(boolean). This changes he overlay color and elevates the card to convy motion 

Note the actual view hierarchy present under MaterialCardView is NOT guaranteed to match teh view hierarchy as written in XML . As a result, calls to getParent() on children of the MaterialCardView, will not return the Material CardView itself, but rather an intermediate View. If you need to access a MateriaclCardView directly, set and android id and use findViewByid. 


