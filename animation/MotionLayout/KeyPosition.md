# KeyPosition
There are three diferent types of keyPosition possible: parentRelative, pathRelative and deltaRelative. Specifying a type will change the coordinate system by which percentX and percentY are calculated. 

## What is a coordinate system
A coordinate system gives a way to specify a point in space. They are useful for describing a poisition on the screen. MotionLayout coordinate systems are a catesain coordinate system. This means they have an X and Y axis defined by two perpendicular lines. The key difference between them is where on the screen the X axis goes, the Y axis is always perpendicular. 

All coordinates system in a MotionLayout use values between 0.0 and 1.0 on the both the X and Y axis. They allow negative values, and values larger than 1.0. For example an percentX value of -2.0 would mean, go in the opposite direction of the X axis twice. 

The KeyPositionType of parentRelative uses the same coordiante system as the screen. It defines (0,0) to the top left of the entire Motionlayout, and (1,1) to the bottom right. 

You can use parentRelative whenever you want to make it curve just a little bit, the other two coordiante system are a better choice. 

Delta is a math term for change, so deltaRelative is a way of saying "change relative". In deltaRelative coordinates(0,0) is the starting position of the view and (1,1) is the ending postion. The X and Y axis is aligned with the screen. 

The X axis is alwasy horizontal on the screen, and the Y axis is always vertical on the screen. Compated to parentRelative, the main difference is that the coordiantes describe just the part of the screen in which the view will be moving. 

DeltaRealive is a greater coordiante system for control horizontal or vertical motion in isolation. For exlample you could create an animation that completes just its vertical movement at 50% and continues animation horizontally. 

Coordinates in deltaRelative are relative to the motion of the veiw. So if the view moves to the right, X will go to the right. If the view moves to the left, X will go to the left. Similarly, the Y dimesnion will follow the view and go up or down. Of course, you can always use negative values or values larger than 1.0 if you want the view to bounce past the start or stop position! 

The coordiantes of deltaRelative will expand or shrink based on the distance the view moves in both directions. So if you have a view that doesn't move very much, or at all in one direction, deltaRelative won't create a useful coordinate system in that direction. For example, a view that moves horizonatlly from right to left will not have a usefull Y axis in deltaRelative. 

## PathRelative 
Is quite different than the other two as the X axis follows the motion path from start to end. So (0,0) is the starting position, and (1,0) is th eending position. 

PathRelative is really useful for the following
- Speeding up, slowing down, or stopping a view during part of the animation. Since the X dimesions will always match the path the view takes exactly, you can use a pathRelative KeyPosition to change which framePosition as a particular point in the path is reached. So a KeyPosition at framePosition="50" with a percentX = "0.1" would cause the animation to take 50% of the time to travel the first 10% of the motion
- Adding a subtle arc to a path. Since the Y dimesnion is always perpendicular to motion, changing Y will change the path to curve relative to the overall motion
- Adding a second dimension  with deltaRealtive won't work. For a compltelely horizontal and vertical motion, deltaRelative will only create on useful dimension. However, pathRelative will always create a usable X and Y coordiantes. 

It's imporant to note that pathRelative will always define a coordinate system in terms of the motion, not of the screen. This means it may be an angle to the overall screen. If you need to modify the horizontal or vertical motion interms of screen coordiantes , one of the other coordiantes system is a better choice. 

A KeyPositoin always defines an (x, y, width, height) position that the View must move through during the transition from start to end. If you don't specify one dimension, it will have default value(zero, or an approperiate value based on the framePosition) .
