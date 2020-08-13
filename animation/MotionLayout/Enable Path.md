#Enable Path

Enable path debugging, This helps develope complex animation with MotionLayout, you can draw the animation path of every view. THis is helpful when you want to visulaize your animation, and or fine tune detals of the animation. 

- Circles: represent teh start or end position of one View
- Lines represent the path of one view. 
- Diamond: represent a keyPosition that modifies the path

All animation in MotionLayout are defined by a start and and end ConstraintSet that defines what the screen looks like before the animation is done. By default, MotionLayout will plot a linear path between the start and end positions of each view that changes position


