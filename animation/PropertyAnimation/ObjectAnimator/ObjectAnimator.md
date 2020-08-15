# ObjectAnimator

This subclass of ValueAnimator provides support for animating properties on a target object. The constructors of this class take parameteres to define the targe object that will be animated as well as the name of the property that will be animated. Approperiate set/get functions are then deteremined interenally and the animation will call these functions as necessary to animate the property.

## Instanate
ObjectAnimator is similar to a ValueAnimatoe, but you specify the object and the name of that object property(as a string) along with the values to animate. 
```
val animate = ObjectAnimator.ofFloat(textView, "traslationX", 100f).apply {
  duration = 1000
  start()
}
```

## Animation
- translationX and translationY: These properties control where the View is located as a delta from its left and top coordinates which are set by its layout container.
- rotation, rotationX, and rotationY: These properties control the rotation in 2D (rotation property) and 3D around the pivot point.
- scaleX and scaleY: These properties control the 2D scaling of a View around its pivot point.
- pivotX and pivotY: These properties control the location of the pivot point, around which the rotation and scaling transforms occur. By default, the pivot point is located at the center of the object.
- x and y: These are simple utility properties to describe the final location of the View in its container, as a sum of the left and top values and translationX and translationY values.
- alpha: Represents the alpha transparency on the View. This value is 1 (opaque) by default, with a value of 0 representing full transparency (not visible).

## Update properties
To have the ObjectAnimator update properties correctly you must 
- The object requires a setter function. Becuase the ObjectAnimator automatically updates the property during animation you must be able to access the it through the setter method. 
```
  setFoo()
```
If there is no setter you have three options. 
  - Add a setter method to the class. 
  - Use a wrapper class and receive the value with a valid setter method and forwardit to the orignial object.
  - Use a ValueAnimaotr instead. 
  
- If you specify only one value of the values... parameter in one of the ObjectAnimator factory methods, it is assumed to be the ending value of the animation. Therefore, the object property that you are animating must have a getter function that is used to obtain the starting values of the animation. 
```
  getFoo()
```

The getter(if needed) and setter methods of the proepty that you are animating must operate on the same type as the starting and ending values that you specify to ObjectAnimator. For example, you must have targetObject.setProperNmae(float and targetObject.getPropName() if you construct the following ObjectAnimator. 
```
ObjectAnimator.ofFloat(targetObject, "propName", 1f)
```

- Depending on what property of object you are animating, you might need to call the invalidate() on a view to force the screen to redraw itself with the updated animated values. You do this in the onAnimationUpdate() callback. For eaxmple, animating the color property of a drawable object only causes updates to the screen when that object redrarws itself. All of the property setters on the View, such as setAlpha() and setTranslationX() invalidate the View properly, so you do not need to invalidate the View when calling theses methods with new values. For more information on listeners, see the section about Animation Listeners. 








