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

When the value animator is done calculating an elapsed fracti9on, it calls the TimeInterpolator that is currently set, to calculate an interpolated fraction. An interpolated fraction maps the elapsed fraction to a new fraction that takes into account the time interpolation that is set

When the interpolated fraction is calculated, ValueAnimator calls the appropriate TypeEvaluator, to calculate the value of the property that you are animating, based onthe interpolated fraction, the starting value, and the ending value of the animation 
