# ObjectAnimator

This subclass of ValueAnimator provides support for animating properties on a target object. The constructors of this class take parameteres to define the targe object that will be animated as well as the name of the property that will be animated. Approperiate set/get functions are then deteremined interenally and the animation will call these functions as necessary to animate the property 

Constructors

```
val animator = ObjetAnimator()
```


## KeyFrame
Using keysframes allows animation to follow more compelx paths from the start to the end values. Not that you can specify explicit fractional values (from 0 to 1) for each keyframe to determine when, in the overall duration, the animation should arrive at that value. Alterantively, you can leaeve the fractions off and the keyframes will be equally ditributed within the total duration, the animation should arrive at that value.


## Interpolator
The interpolator will be applied on the interval betwen the keyframe that the interpolator is set on and the previous keyfram. When no interpolator is upplied the default Accelerate DeclaratInterpolatro will be used. 




