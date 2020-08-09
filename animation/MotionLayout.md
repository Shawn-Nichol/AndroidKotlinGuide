- A MotionLayout: which is a subclass of ConstraintLayout. You specify all the views to be animated inside the motionLayout tag.
- A MotionScene which is an XML file that descibes an animation for MotionLayout.
- A transition which is part of the MotionScene that specifies the animation duration, trigger, and how to move the views. 
- A ConstraintSet that  specifies both the start and the end consntraints of the transition. 

A Motion Layout is a subclass of Constraintlayout, so it supports all the same features while adding animation. To use MotionLayotu, you use a MotionLayout view where you would use ConstraintLayout. 

Start: descibes what the screen looks like before the animation, 
End: descibes what the screen looks like after the animation has completed.

MotionScene uses a ConstraintSet tag to define the start and end states. A ConstraintSet is what is ounds like, a set of constraints that can be applied to views. This includes width, height and ConstraingLayout constrains. It also includes some attributes such as alpha. It doesn't contain the views themselves, just the constraints on those views.

Any constraint specified in a ConstraintSet will override the constraints specified in the layout file. If you define constraints in both the layout and the MotionScence, only the constraints in the MotionScene are applied. 

ConstraintSet is a child of MotionScence that defines a set of constraints that wil be applied t one or more views declared in your layout file. These constraints can be used to define the start of the end of an animation. 
 Constraints sets contain only contain the constraints of layout information such as width, height, alpha or visiblity. They Don't contian other information such as the type of view or any view specific attributes like text on a  TextView. 
 
 
 Views that are not animated by a motionlayout animation should specify their constraints in the layout XML file. MotionLayout will not modify constraints that aren't referenced by a <Constraint> in the <MotionScene>. 
 Views that are animated should have their constraints set in the motion scence XML file. 
 
## onSwipe
OnSwipe contains a few aattibutes, that most important being touchAnchorId
- touchAncorId: is tracked view that moves in response to touch. MotionLayout will keep this view the sam distance from the finger that's swipping. 
- touchAnchorSide: Determines which side of the view should be tracked. This is important for views that resize, follow complex paths, or have one side that moves faster than the other. 
- dragDirection: determines which directino matters for this animation (up, down, left or right

When MotionLayout listens for drag events, the listener will be registereed on teh MotionLayout view and no the view specified by touchAnchorId. Whne a user starts a gesture anywhere on the screen, MotionLayout will keep the distance between their finger and the touchAnchorsSide of the touchAnchorId view constant. If they touch 100 dp away from the anchor side, for example, MotionLayout will keep that side 100dp away from their finger for the eniter animation. 

OnSwipe listens for sipes on teh MotionLayout and no the view specified in touchAncourId. This means the user may sipe outside of the specified view to run the animation. 
