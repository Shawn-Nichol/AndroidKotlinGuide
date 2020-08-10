# onSwipe
OnSwipe Attributes.
- TouchAndroid: is tracked view that moves in response to touch. MotionLayout will keep this view the same distance from the finger that's swiping
- TouchAncorSide: Determines which side of the view should be tracked. THis important for views that resize, following complex paths, or have one side that moves faster than the other. 
- dragDirection: determine which direction matters for this animation(up, down, left, right). 

When MotionLayout listens for a drag event, the listener will be registeredd on the MotionLayout view and no the view dpecified by touchAndroid. When a user starts a geseture anywhere on the screen. MotionLayout will keep the distance between their finger and the  touchAnchorsSide of the touchAnchorId view constant. If they touch 100 dp away from the anchor side, for example, MotionLayout will keep that side 100 dp away from their finger for the eniter animation. 

OnSwipe listens for swipes on the MotionLayout and now the view specified in touchAndroid. This means the user may swipe outside of the specified view to run the animation. 


