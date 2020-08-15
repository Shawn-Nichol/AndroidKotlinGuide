## Motion Layout
A MotionLayout is a subclass of ConstraintLayout, You specify the views to be animation inside the MotionLayoutTag. A MotionLayout has the following atatributes. 
- MotionScene: XML file that descibes an animation for MotionLayout
- A transition: which is part the MotionScence
- A constraint set that specifies the start and end constraints. 

## Property Animation
Property animation allows you to animate any object over a specifed length of time. You can set the following attributes for the animations
- Duration: The length of the animation.
- Time interpolation: Specify the length of certain animations
- Repeat Count: Specfiy the amount of times the animation runs
- Animator sets: Allows you to gropu animation into logical sets that play together or sequentially
- FrameRefresh: Specify how often to refresh the frames of the animation. 

Advantages:
- You can animate any object an propety
- The object is modified which means you would click the button that has moved instead of the space where the button was before it moved. 
- It is more robust then ViewAnimation, you can assign animatiors to the properties that you want such as color position, or size and you can define aspects of the move. 



## View Animation
Preforms a tween animation, Tween animation can perform the following transformation, position, size, rotation, and trasparency on the content of a View Object.

Advantages:
- The View animation takes less time to setup and requires less code to write. 

Disavantages:
- You cannont animate properties such as color. 
- It is only modified where the View wwas drawn and nothe acutal view itself. If a button moves to a new location a click will not take place at the new location, instead it will take place at the orignal draw location





