# PropertyAnimation
The property animation system is a robust framework that allows you to animate almost anything. You can define an animation to chagne any object property over time, regardless of whether it draws to the screen or not. A property animation changes a propert's value over a specified length of time. To animate something, you specify the object property that you want to animate, such as an object's position on the screen how long you want to animate it for and what values you want to animate between. 

The property animation system lets you define  the following characteristics of animations
- Duration: you specify the duration of an aanimtaitoin. Defualt is 300 ms
- Time interpolations: You can specify how the values for the property are calculated as a function of the animations current elapsed time. 
- Repeat count and behavior: You can specify whether or not to have an aniation repeat when it reaches the end of a duration and how many times to repeat the animation. You can also specify whether you want the animation to play back in reverse. Setting it to reverse plays the animation fowards then backwards repeatedly, until the number of repeats is reached. 
- Animators sets: You can group animations into logical sets that play toether or sequentially or after specified delays
- Frame refresh delay: you can specify how often to refresh frames of your animation. The default is set to refresh every 10ms, but the speed in which you application can refresh frames is ultimately dependent on how busy th esystem is overall an d how fast the system can servcie the underlying timer. 




## ValueAnimator
The main timing engine for property animation that also computes the values for the property to be animated. It has all of the core functionality that calculates animation values and contains the timing details of each animation, information about whether an animation repeats, listeners that recieve update events, and the ability  to set custom types to evaluate. There are to pieces to animating properties: calculating the animated values and setting those values on the object of a property that is being animated. ValueAnimator does not carry out the second piece, so you must listen for updates to calutes by the ValueAnimator and modify the objects that you want to animate with your own logic. 

## ObjectAnimator
Is a subclass of ValueAnimator that allows you to set a property of a target object to animate. This class updates the property accordingly when it computes a new value for the animation. You want to use ObjectAnimator most of the time, becuase it makes the process of animating values on target objects much easier. However you sometimes want to use ValueAnimator directly becuase ObjectAnimator has a few more restrictions, such as requiring specific accessor methods to be present on the target object. 

## AnimatorSet
Provides a mechanism to group animations together so that they run in relation to one antoerh. You can set animations ot play toegether, sequentially, or after a specified delay. 
