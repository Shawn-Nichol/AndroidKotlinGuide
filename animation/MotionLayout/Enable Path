Enable path debugging, This helps develope complex animation with MotionLayout, you can draw the animation path of every view. THis is helpful when you want to visulaize your animation, and or fine tune detals of the animation. 

- Circles: represent teh start or end position of one View
- Lines represent the path of one view. 
- Diamond: represent a keyPosition that modifies the path

All animation in MotionLayout are defined by a start and and end ConstraintSet that defines what the screen looks like before the animation is done. By default, MotionLayout will plot a linear path between the start and end positions of each view that changes position

### KeyPosition
KePosition Modifeis the path a view takes between the start and end constraints. 

It can distor the path of a view to go throught a thrid point between the start and end positions. Or it can even speed up and sloww down progress along either the x or y axis. A Key Position can only change the path during the animation, it cannot change the start or the end. The ConstraintSet will always specfiy the final position for the views at the start and end of the animation. 

A KeyFrameSet is a child of a Transistion, and it is set of all the KeyFrames, such as Key Position, that will be applied during the transition. As MotionLayout is calculating the path for the moon between the start and the end, it modifies the path based on teh KeyPosition specified in the KeyFrameSet. A keyPositio has several attributes that descibe how it modifies, the most important ones are
- framePosition is a nujmber between 0 and 100 to say when in the animation this KeyPosition should be applied, whith 1 being 1% through the animation 99 being 99% through the animation. So if the value is 50 you apply it in the middle of the animation
- motionTarget is the view for which this keyposition modifies the path
- KeyPosionType is how this KeyPosition modifies the path. It can be either parentRelativ, pathRelative, or deltaRelative 
- percentX | percentY is how much to modfy the path at frame Postionvalues between 0.0 and 1.0 with negative values nad  values > 1 allowed

You can think of it this way, At framePosiiont modify the path of teh motionTarget by moving it by the percentX or percentY according to the coordinates deteremined by teh keyPositionType.

By default MotionLayout will round any corners that are introduced by modifying the path.
