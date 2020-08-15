The property animation system allow streamlined animation of View objects and offers a few advantages over the view animation system. The View animation system transformed View objects by changing the way that they were drawn. This was handled in the continer of each View, becuase the View itself had no properties to manipulate. This led to behavior such as an object still existing in its original location, even though it was drawn on a different location on the screen. 

The prperty animation system can animate Views on teh screen by changing the  actual properties in the View objects. In additions, Views also automatically call the invalidate() to reresh tthe screen whenever it properties are changed. The new properites in teh Vieww class that facilitate prperty animations are
- TranslationX and translationY: These preoprties control whwere the View is located as a delta from its left and top coordinates hwich are set by its layoutconatainer. 
- rotation, rotationX and rotationY: These properties contorl the rotation in 2D and 3d around teh pivot point
- scaleX and scaleY: thesee proeprties control the 2d scaling of a View around its pivot point
- pivotX and pivotY: These properties control the location of the piovt point, around which the rotation and scaling transforms occcur. By default the pivot point is located at the center of the object
- x and y: These are simple utility properties to descibe the final location of the View in its container as a sum of the left and top values and translationX and translationY values. 
- Alpha represents the alpha transparency of the View. This value is 1 (opaque) by default, whith a value of 0 representing transparency (not visible). 

To animate a property of a View object, sush as its color or rotation value, all you need to do is create a property animator and specify the View property that you want to animate. 
```
ObjectAnimator.ofFloat(myView, "rotation", 0f, 360f)
```
