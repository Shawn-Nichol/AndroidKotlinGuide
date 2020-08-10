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

## Enable path
enable path debugging, This helps develop complex animations with MotionLayout, you can draw the animation path of every view. This is helpful when you want to visualize your animation, and ofr fine tuning the small details of motion. 

Run the app to path of animation
- Circles represent the start or end position of one view
- Lines represent the path of one view
- Diamonsd represent a keyPosition that modifies the path

All animation in MotionLayout are defined by a start and an end ConstraintSet that defines what the screen looks like before the animtaion is done. By default, MotionLayout will plot a linear path between the start and end positions of each view that changes position.

KeyPosition: modifies the path a view takes between the start and end ConstraintSet. 

It can distort the path of a view to go through a third point between the start and end positions. Or it can even speed up and slow down progress along either the s or y axis. 
A KeyPosition can only change the path during the animation, it cannot change the start or the end. The ConstraintSet wil always specify the final position for the views at the start and end of the animation.

A KeyFrameSet is a child of a Transition, and it is set of all the KeyFrames, such as KeyPosition, that should be applied during the transition. 
As MotionLayout is calculating the path for the moon between the start and the end, it modifies the path based on the KeyPosition specified in the KeyFrrameSet. You can see how this modifies the path by running the app again. 
A KeyPosition has several attributes that describe how it modifies the path, the most important ones are
- framePosition is a number between 0 and 100 to say when in the animation this KeyPosition should be applied, with 1 being 1% through the animation and 99 being 99% through the animation. So if the value is 50 you apply it in the middle of the animation. 
- motionTarget is the view for which this keypositioin modifies the path
- keyPositionType is how this KeyPosition modifies the path. It can be either parentRelative. pathRelative, or deltaRelative
-percentX | percentY is how much to modify the path at framePositionvalues between 0.0 and 1.0, with negative values and values > 1allowed

You can think of it this way , At framePosition modify the path of the motionTarget by moving it by the percenetX or percentY according to the coordinates determined by the keyPositionType

By default MotionLayout will round any corners that are introduced by modifying the path. If you look at the animation you just created, you can see the moon follows a curved path at the bend. For most animations, this what youwant, and if not you can specify the curveFit attribute to customei it. 


## KeyPosition
There are three different types of keyPosition possible: parentRelative, pathRelative and deltaRelative. Specifying a type will change the coordinate system by which percentX and percentY are calculated

### What is a coordinate system
A coordiante system gives a way to specify a point in space. They are also useful for describing a position on the screen. 
MotionLayout coordinate systems are a catesain coordiante system. This means they have an X and a Y axis defined by two perpendicular lines. The key differnece between them is where on the screen the X axis goes, the Y axis is always perependicular. 

All coordiante systems in the MotionLayout use values between 0.0 and 1.0 on the both the X and Y axis. They allow negative values, and values larger than 1.0. So for example an percentX value of -2.0 would mean , go in the opposite direction of the X axis twice. 

The keyPositionType of parentRelative uses the same coordiante system as the screen. It defines (0,0) to the top left of the entire Motionlayout, and (1,1) to the bottom right. 

You can use parentRelative whenever you want to make it curve just a little bit, the other two coordiante system are a better choice. 

Delta is a math term for change, so deltaRelative is a way of saying "change relative". In deltaRelative coordinates(0,0) is the starting position of the view, and (1,1) is the ending position. The X and Y axes is aligned with the screen. 

The X axis is always horizaontal on the screen, and the Y axis is always vertical on the screen. Compared to parentRelative, the main difference is that the coordiates describe just the part of the screen in which the view will be moving

deltaRealtive is a greater coordiante system for control horizontal or vertical motion in isolation. For example you could create an animation that completes just its vertcail movement at 50% and contiuse animation horizonatlly. 


Coordiantes in deltaRelative are relative to the motion of the view. 
So if the view moves to the right, X will go to the right. If the view moves to the left, X will go to the left. Similarly , the Y dimension will follow the view and go up or down. 
Of course, you can always use negative values or values larger than 1.0 if you want the view to bounce past the start or stop position!

The coordiantes of deltaRelative will expand or shrink based on the distance the view moves in both directions. 
So, if you have a view that doesn't move very much, or at all, in one direction, deltaRelative won't create a useful coordiante system in that direction. For example, a view that moves horizontally from the right to left will not have a useful Y axis in deltaRelative. 


## PathRelative
Is quite different than the other two as the X axis follows the  motion path from start to end. So (0,0) is the starting position, and (1,0) is the  ending position. 

Why would you want this? it's quite surprising at first glance, especially since this coordiante system isn't even aligned to the screen coordiante system. 

It turns out pathRelative is really useful for a few things
- Speeding up, slowing down, or stopping a view during part of the animation. Since the X dimension will always match the path the view takes exactly, you can use a pathRelative KeyPosition to change which framePosition a particular point in that path is reached. So a KeyPosition at framePosition="50" with a percentX="0.1" would cause the animation to take 50% of the time to travel the first 10% of the motion. 
- Adding a subtle arc to a path. Since the Y dimension is always perpendicular to motion,changing Y will change the path to curve relative to the overall motion
- Adding a second dimension whe deltaRelative won't work. For completelly hotizontal and vertical motion, deltaRelative will only create one useful dimension. However, pathRelative will always create usable X and y coordinates. 

It's important to note that pathRelative will always define a coordiante system in terms of the motion, not of the screen. 
This means it may be an angle to the overall screen. If you need to modify the horizontal or vertical motion in terms of screen coordiates, one of the other coordinate system is a better choice. 


###
A keyPosition alwasy defines an (x, y, width, height) poistion that the View must move through during the transition from start to end. 
If youdon't specify one dimension, it will have a default value(zero, or an approperiate value based on the framPosition). In this example, by specifying only percentY, the percentX keeps its default value for that keyPosition


## Changing attributes during animation
Building dynamic animations often means changin the size, rotation, or alpha  of a views as the animatin progresses. MotionLayout supports animating many attributes on a any view using a keyAttribute. 

## Custom Attribute
You can use a CustomAttribute to specify any other attribute.  A customAttribute can be used to set any value that has a setter. For example you can set the backgrouncColor on a View using a customAttribute. MotionLayout will use reflection to find the setter, then call it repeatedly to animate the view. 

Custom attribute is added inside a KeyAttribte. The custom Attribute will be applied at the framePosition specified by the KeyAttribute. 
inside the CustomAttribute you must specify an attributeName and one value to set
- app:attrivuteName is the anme of the seeter that will be called by this custom attribute. 
- app:custom*value is a custom value of the type noted in the name, in this example the custom value is a color specfied. 
CustomValues can have any of the following. 
- color
- Integer
- Float
- String
- Dimension
- Boolean

As long as Motionlayout can find a setter that takes the correct typek it can animate changes between values. 

## Drag events
The touchancourside passed to OnSwipe must progress in a single direction through the entire animation. If the anchored side reverses its path, or pauses, MotionLayout will get confused and not progress in a smooth motion. 
In some animations, no view has an approperate touchAnchor side. This may happen if every side follows a complex path through the motion, or views resize in ways that would cause surprising animations. In these stiuation consider adding an invisible view that follows a simpler path to track. 

You can also combine dragDirection with touchAnchorSide to make a side track a different direction than it normally would. It's still important that the touchAnchorSide only progress in one driection, but you can tell MotionLayout which direction to track. For example you can keep the touchAnchorside="bottom, but add dragDirection="dragRight". This will cause MotionLayout to track the position of the bottom of the view, but only consider its location when moving right. So even through the bottom goes up and down, it will still animate correctly with onSiwpe. 

