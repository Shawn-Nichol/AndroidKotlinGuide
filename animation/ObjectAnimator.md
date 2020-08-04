## ObjectAnimator

This subclass of ValueAnimator provides support for animating properties on a traget object. The constructors of this class take parameters to define the target object that will be animated as well as the name of the property that will be animated. Appropeeriate set/getfunctions are then determined internally and the animation will call theses functions as necessary to animate the property

Animators can be created from either code or resource files.

```
<objectAnimator xlmsn:android="http://schemas.android.com/apk/res/android"
    android:duration="1000"
    android:valueTo="200"
    android:valueType="floatType"
    android:propertyName="y"
    android:repeatCount="1"
    android:repeatMode="reverse"/>
```

Using keyframes allows animations to follow more complex paths from the start to then end values. Note that you can specify explicit fractional values (from 0 to 1) for each keyframe to determine when, in the overall duration, the animation should arrive at that value. Alterantively, you can leave the fractions off and the keyframes will beequally distributed within the total duration, the animation should arrive at that value. Alternatively, you can leave the fractions off and the keyframes will be equally distrbuted within the total duration. Also , a keyframe with no value will derive its value optional interpolator can be specified. The interpolator will be applied on the interval between the keyframe that the interpolator is set on and the previous keyfram. When no interpolator is upplied, the default AccelerateDeclerateInterpolatro will be Used. 
