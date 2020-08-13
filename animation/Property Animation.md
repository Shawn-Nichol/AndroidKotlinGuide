


## Animate views
Teh property animation system allows streamlined animation of View objects and offers a few advantages over the view animation system. The view animation system transformed View objects by changing the way that they were drawn. This was handled in the container of each View, becuase the View itself had no properties to manipulate. This resulted in the View being animated, but caused no chnage in teh View object itsefl. This led to behavior such as an object still existing in its originnal location, even though it was drawn on a different location on the screen. In Android 3.0 new properties and the corresponding getter and setter mthods were added to eliminate this drawback. 

The property animation system can animate Views on the screen by changin the actual properties in the View objects. In addition, Views also automaticallyl call the invalidate() method  to refresh the screen whenever its properties are changed. The new properties in teh view class that facilitatte property animations are
- TranslationX and TranslaytionY: These properties control where the view is lcoated as a delta from its left and top coordinates which are set by its layout container. 
- rotation, rotationX and rotationY: These properties control the rotation in 2D and 3d aroudn the pivot point. 
- scaleX and scaleY: These properties control the 2d scaling of a view around its pivot point
- pivotX and pivot Y: These properties contorl the location of the pivot point, around wich the rotation and scaling transformationoccur. By default the pivot point is lcoated at the center of the object
- x and y theses are simple utlitily properties to describe the final ocation of the View in its container, as a sum of the left and top values and translationX and traslationY values
- alapha Represents the alpha transparency on the view. This value is 1 by default with a value of 0- representing full transparency 

To animate a property of a view object, such as its color or rotation value, all you need todo is create a property animator and specify the View property that you want to animate.

## Animate using ViewpRopertyanimator
The ViewpRopertyAnimator provides a simple way to animate several properties of a View in parallel, using a single underlying Animator object. It behaves much liek an ObjectAnimator, becuase it modifies the actual values of the view's properties, but is more efficient when animating many properties at once. In addition the code for using the ViewPropertyAnimator is much more concise and easier to read. The following code snippets show the differences in simultaneously animating the x and y property of a view. 

## Potential effects on UI performance
Animators that update the UI cause extra rendering work for every frame in which the animation runs. For this reason, using resource intensive animations can negatively impact the performacne of your app

Work required to animte your UI is added to the animation stage of teh rendering pipeline. YOu can find out if your animations impact the performacne of your app by enbaling Profile GPU rendering and monitoring the animation stage
