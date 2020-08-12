## ValueAnimator
The main timing engine for property animation that also computes the values for the property to be animated. It has all of the core functionality that calculates animation values and contains the timing details of each animation, information about whether an animation repeats, listeners that recieve update events, and the ability  to set custom types to evaluate. There are to pieces to animating properties: calculating the animated values and setting those values on the object an property that is being animated. ValueAnimator does not carry out the second piece, so you must listen for updates to calues calculated by teh ValueAnimator and modify the objects that you want to animate with your own logic. 

## ObjectAnimator
A subclass of ValueAnimator that allows you to set a target object an object propety to animate. This class updates the property accordingly when it computes a new value for the animation. You want to use ObjectAnimator most of the time, becuase it makes the process of animating values on target objects much easier. However you sometimes want to use ValueAnimator directly becuase ObjectAnimator has a few more restrictions, such as requiring specific accessor methods to be present on the target object. 

## AnimatorSet
Provides a mechanism to group animations together so that they run in relation to one antoerh. You can set animations ot play toegether, sequentially, or after a specified delay. S
