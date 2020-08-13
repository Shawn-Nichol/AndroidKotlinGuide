# Enable Path

Enable path debugging, This will provide a visual path of the animation, you can draw the animation path for every view. This helpful when you want o visulaize your animation, or fine tune the small details of the animation.

Path symbols
- Circles: represent the start or end position of one view. 
- Lines represent the path of one view.
- Dimaonds: represent a KeyPosition that modifies the path. 

All aniammtion in MotionLayout are defined by a start and end constraintSet that defines what the screen looks like before the animation is done. By default, MotionLayout will plot a linear pth between the start and end positions of each view that changes positions. 

KeyPositions: Modifies the path a veiw takes between the start and end ConstraintSet. 

It can distort the path of a view to go through a thrid point between the start and end positions. Or it can even speed up and slow down progress along either the x or y axis. A KeypPosition can only change the path during the animation, it cannot change the start or the end. The ConstraintSet will alwasy specify the final position for the views at the start and end of the animation. 

A KeyFrameSet is a child of a Transition, and it is sest of al lthe KeyFrames, such as KeyPosition, that should be applied during the transition. As MotionLayout is calculating the path for the moon between the start and the end, it modofies the path based on the KeyPosition specified in the KeyFrameSet. A KeyPosition has several attributes that desciabe how it modifies the path, the most important one are. 

- framePosition: is a number between 0 and 100 to say when in the animation this KeyPosition should be applied, with 1 being 1% through the animatino and 99 being 99% throught the animation. So if the value is 50 you apply it in the middle of the animation. 
- motionTarget: is the view for which this KeyPosition modifies the path
- KeyPositionType: is how this KeyPosition modifes the path. It can beeither parentRelative,  pathRealtive, or deltaRealtive 
- PercentX | PercentY: is how much to modify the path at framePositionvalues between 0.0 and 1.0 with engative values and values > 1 allowed. 

A framePosition modifes the path of the motionTarget by moving it by thee percentX or percentY according to the coordinates determined by teh KeyPositionType. 

By default MotionLayout will round any corners that are introduced by modifying the path.

