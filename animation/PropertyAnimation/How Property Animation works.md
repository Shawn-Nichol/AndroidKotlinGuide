# How Property Animation works
The valueAnimator object keeps track of your animation's timing, such as how long the animation has been running, and the current values of the property that it is animating. 

The valueAnimator encapsulates a TimeInterpolator, which defines animations interpolation, and a TypeEvaluator used would be AccelerateDecelarateInterplation and the TypeEvaluator would be InEvalutor

To start an animation, create a valueAnimator and give it the starting and ending values of the property that you want to animate, along with the duration of the animation. When you call start() the animation begins. During the whole animation, the ValueAnimator calculates an elapsed fraction between 0 and 1 based on the duration of the animation and how much time has elapsed. The elapsed fraction represents the percentage of time that the animmation has completed, 0 meaning 0% and 1 meaning 100%. 

When the value animator is done calculating an a elapsed fraction, it calls the TimeInterpolator that is currently set, to calculate an interpolated fraction. An interpolated fraction swaps the elpased fraction to a new fraction that takes into account the time interpolation is set. 

When the inerpolated fraction is calulated, ValueAnimator calls the appropriate TypeEvaluator, to calculate the value of the property that you are animating, based on the interpolated fraction, the starting value, and the ending value of the animation. 

