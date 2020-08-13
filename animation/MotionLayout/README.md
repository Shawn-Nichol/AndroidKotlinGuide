# MotionLayout
- MotionLayout: is a subclass of ConstraintLayout> You specify all the views to be animated inside the motionLayout tag. 
- MotionScene which is an XML file that descibes an animation for MotionLayout. 
- A transistion which is part of the MotionScene that specifies the animation duration, trigger and how to move the views. 
- A constraintSet that specifies both the start and the end constraints of the transistion.

A MotionLayout is a subclass of ConstrainLayout, so it supports all the same features while adding animation. Use a MotionLayout view where you would use ConstrainLayout. 

Start: desceibes what the screen looks like before the animation
End: Descibes what the screen looks like after the animation has completed. 

MotionScene uses a ConstraintSet tag to define the start and end states. A ConstraintSet is a set of constraints that can be applied to views. This includes width, height and ConstraintLayout constraints. It also includes some attributes such as alpha. It doesn't contain the views themseelves, just the constraints on those views. 

Any constraint specified in a ConstraintSet will override the constraints specified in the layout file. If you define constraints in both the layout and the Motionscene, only the constraints in teh Motionscene are applied. 

ConstraintSet is a child of MotionScene that defines a set of constraints that will be applied to one or more views declared in the layoutfile. These constraints can be used to define the start of the end of an animation. Constraints sests contain only contain the constraints of layout information such as width, height, alpha or visiblity. THey don't dontain other information such as the type of view or any view specific attributes like text on a TextView. 

Views that are not animated by a MotionLayout animation should specify their constraints in teh layout XML file. MotionLayout will not modify constraints that aren't referenced by the MotionScene. Views taht are animated shoould have their constraints set in teh motion scene XML file. 
