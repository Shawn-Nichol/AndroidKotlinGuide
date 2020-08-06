The property animation system is a robust framwork that allows you to animate almost anything. You can define an animation to change any object property over time, regardless of whether it draws to the screen or not. A property animation changes a property's value over a specified length of time. To animate something, you specify the object property that you want to animate, such as an object's position on the screen, how long you want to animate it for, and what values you want to animate between.

The property animation system lets you define the following characteristics of animation:
- Duration: you can specify the duration of an animation. The default length is 300ms.
- Time interpolations: You can specify how the values for the property are calculated as a function of the animations current elapsed time.
- Repeat count and behavior: You can specify whether or not to have an animation repeat when it reaches the end of a duration and how many times to repeat the animation. You can also specify whether you want the animation to play back in reverse. Setting it to reverse plays the animation forwards then backwards repeatedly, until the number of repeats is reached. 
- Animator Sets: You can group animations into logical sets that play together or sequentially  or after specified delays.
- Frame refresh delay: You can specify how often to refresh frames of your animation. The default is set to refresh every 10 ms, but the speed in which you application can refresh frames is ultimately dpenedent on how busy the system is overall and how fast the system can service the underlying timer. 

## How Property Animation works
The valueAnimator object keeps track of your animation's timing, such as how long the animation has bee running, and the current value of the property that it is animating

The valueAnimator encapsulates a TimeInterpolator, which defines animations interpolation, and a TypeEvaluator used would be AccelerateDecelarateInterpolationn and the TypeEvaluator would be InEvaluator.

To start an animation, create a ValueAnimator and give it the starting and ending values for the proprety that you want to animate, along withthe duration fo the animation. When you call start() the animation begins. During the whole animation, the ValueAnimator calculate an elapsed fraction between 0 and 1 based on the duration of the animation and how much time has elapsed. Teh elapsed fraction represents the percentage of time that the animation has completed, 0 meaning 0% and 1 meaning 100%. 

When the value animator is done calculating an elapsed fraction, it calls the TimeInterpolator that is currently set, to calculate an interpolated fraction. An interpolated fraction maps the elapsed fraction to a new fraction that takes into account the time interpolation that is set

When the interpolated fraction is calculated, ValueAnimator calls the appropriate TypeEvaluator, to calculate the value of the property that you are animating, based onthe interpolated fraction, the starting value, and the ending value of the animation 


## How property animation differs from view animation
The view animation system provides the capability to only animate View objects, so if you wanted to animate non-View objects, you have to implemnet your own code to do so. The view animation system is also constrained in the fact that it only exposes a few aspects of a View object to animate, such as the scaling and rotation of a View but not the background color, for instance.  

Another disadvantage of the view animation system is that it only modified where the View awas drawn, and not the actual View itself. For instance if you animated a button to move across the screen, the button draws corrrectly, but the actual location where you can click the button does not change, so you have to implement your own logic to handle this. 

Whit the property animation system, theses constraints are completely removed, and you can animate any property of any object and the object itself is actually modified. The property animation system is also more robust in the way it carries out animation. At a high level, you assign animators to the properties that you want to animate, such as color position, or size and cna define aspsects of the animation such as interpolation and synchronization of multiple animators. 

The view animation system, however, takes less time to setup and requires less code to write. If view animation accomplishes everything that you need todo, or if yourexisting code already works the way you want, there is no need to use the propertyu animation system. It also might make sense to use both animation systems for different situations if the use case arises.

## API OverView
You can find most of the property animation system APIs in android.animation. Becuase the view animation system already defines many interpolators in android.view.animation, you can use those interpolators in the property animation system as well. The following tables describe the main components of the property animation system. 

The animator class provides the basic structure for creating animations. You normally do not use this class directly as it only provides minimal functionality that must be extended to fully support animating values> the following subclasses extend Animator. 

### Value Animator
The main timing engine for property animation that also computes the values for the property to be animated. It has all of the core functionality that calculates animation values and contains the timing details of each animation, information about whether an animation repeats, listeners that receive update events, and teh ability to set custom types to evaluate. There are two pieces to animating properties: calcualting the animated values and setting those values on teh object and property that is being animated. ValueAnimator does not carry out the second piece, so you must listen for updates to values calculated by the ValueAnimator and modify the objects that you want to animate with your own lgoic. 

### ObjectAnimator
A subclass of ValueAnimator that allows you to set a target object and object property to animate. This class updates the property accordingly when it computes a new value for the animation. You want to use ObjectAnimator most of the time, becuase it makes the process of animating values on target objects much easier. However, you sometimes want to use ValueAnimator directly becuase ObjectAnimator has a few more restrictions, such as requiring specific accessor methods to be present on the target object.

### Animator Set
Provides a mchanism to gropu animmations together so that they run in relation to one another. You can set animations to play toether, sequentially, or after a specified delay. 


## Evaulators
Evaluators tell teh property animation system how to calculate values for a given property. They take the timing data that is provided by an Animator class, teh animation's start and end value, and calculate the animated values of the property based on this data. The property animation system provides the following evaluators: 

### TypeEvaluator
An interface that allows you to create your own evaluator. If you are animating an object property that is not an int, float or color, you want to process those types different than the default behavior. 


## Use Interpolators 
An interpolator  defines how specific values in an animation are calculated as a function of time. For example, you can specify animations to happen linearly across the whole animation, meaning the animation moves evenly the entire time, or you can specify animations to use non-linear time, for example using acceleration or decelartion at the beginning or end of the animation

Interpolators in the animation system receive a fraction from Animators that represent teh elapsed time of the animation. Interpolators modify this fraction coincide with the type of animation that it aims to provide. The Android system provides a set of common interpolators in teh android.view.animation package. If none of these suit your needs, you can implmeent the TimeInterpolator interface and create your own. 

As an example how the default interpolator AccelerateDecelerateInterpolator and the LinearInterpolator calculate interpolated fractions are compared below. The linearInterpolator has no effec on the elapsed fraction. Teh accelarationDeclerateInterpolator accelorates into the animation and decelerates out of it. The following methods define the logic for these interpolators. 

```
override fun getInterpolation(input: Float) : Float = 
  Math.cos((input + 1)  * Math.PI) / 2.0f).toFloat() + 0.5f
``` 
  ## Specify keyframes
  A keyframe object consists of a time/value pair that lets you define a specific state at a specific time of an animation. Each keyframe can aslo have its own interpolator to control the behavior of the animation in the interval between the previous keyframe's time and the time of this keyframe
  
  To instantiate a Keyframe object, you must use one of the factory methods, ofInt(), ofFloat(), or ofObject() to obtain teh appropriate type of Keyframe. You then call the ofKeyfram() factory method to obtain a PropertyValuesHolder object. Once you have the object, you can obtain an animator by passing in the PropertyValuesHolder object and the object to animate. The following code snippet demonstrates how to do this
  
```
val kf0 = Keyframe.ofFloat(0f,0f)
val kf1 = Keyframe.ofFloat(0.5f, 360f)
val kf2 = Keyframe.ofFloat(1f, 0f)
val pvhRotation = PropertyValuesHolder.ofKeyframe("rotation", kf0, kf1, kf2)
ObjectAnimator.ofPropertyValuesHolder(target, pvhRotations).apply {
duration = 5000
```
