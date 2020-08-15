# KeyFrames
A Keyframe objec consist of a time/value pair that lets you define a specific time of an animation. Each keyframe can also have its own interpolator to control the behavior of the animation in the interval between the previous keyframe's time and the time of this keyframe. 

To instantiate a Keyframe object, you must use one of the factory methods, ofInt(), ofFloat(), or ofObject() to obtain the appropriate type of Keyframe. You then call the ofKeyframe() factory method to obtain a PropertyValuesHolder object. Once you have the object, you can obtain an animator by passing in the PropertyValuesHolder object and the object to animate.

ex 

```
val kf0 = Keyframe.ofFloat(0f, 0f) 
val kf1 = Keyframe.ofFloat(.5f, 360f)
val kf2 = Keyframe.ofFloat(1f, 0f)
val pvhRotation = PropertyValuesHolder.ofKeyframe("rotation", kf0, kf1, kf2)
ObjectAnimator.ofPropertyValuesHolder(target, pvhRotation).apply {
  duration = 5000
}
```
