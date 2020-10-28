A shader is a type of computer program that was originally used for shading(the productino of appropriate levels of light, darkness, and color witin an image), but which now performs a variety of specialized functions in various fields of computer graphics special effects.

In Android, shader defines the colors of the texture with chich the paint object should draw(other than a bitmap). Android defiens several subclasses of shader for Paint to use, such as BitmapShader, ComposeShader, LinearGradient, RadialGradient, and SweepGradient. 

## Shader
A shader defines teh texture ofr a Paint object. A subclass of Shader is installed in a Paint by calling paint.setShader(shader). After that, any object that is drawn with that Paint will get its colors from the shader. Android provides teh following subclassess of Shader for Paint to use

LinearGradient: draws a linear gradient using two or more given colors

RadialGradient: draws a radial gradient using the given colors, the center, and the radius. The colors are distributed between the center adn edge of the circle. 

SweepingGradient, draws a sweeping gradient around a center point with the specified colors. 

- ComposeShader is a composition of two shaders. ComposeShader and composing modes are beyond

- BitmapShader draws a bitmap drawable as a texture. The bitmap can be repeated or mirrored by setting the TileMode mode. You willl learn more about BitmapShader and TileMode later in this codelab. 

## Concept:  PorterDuff.Mode
The PorterDuff.Mode class provides several Alpha compositing and blending modes. Alpha compositing is the process of compositing a source image with a detination image to create the appearance of partial or full transparency. Transparency is defined by the alpha channel. The alpha channel represents the degree of transparency of a color, that is for its red, green and blue channesl

DST The source pixels are discarded, leaving the destination intact.

DST_ATOP The desetination pixels  that are not covered by source pixels are discarded. 

DST_IN The destination pixels that cover source pixels, are kept and the remaining source and destination pixels are dicarded

DST_OUT the destination pixels that are not covered by source are kept. 


## Tile Mode
The tileing mode, TileMode, defined in the Shader, specifies how the bitmap drawable is repeated or mirrored in the X and Y directions if the bitmap drawable being used for texture is smaller than the screen. Android provides three different ways to repeat(tile) the bitmap drawable. 

- Repeat: Repeats the bitmap shader's image horizontally and vertically.
- CLAMP: The edge colors will be used to fill the extra space outside of the shader's image bounds
- Mirror: The shader's image is mirroed horizontally and vertically. 


## Matrix translation
When the user touches and holds the screen for the spotlight, instead of calculating where the spotlight needs to be drawn, you move the shader matrix; that is, the texture/shader coordinate system, and then draw the texture at the same location in the translated coordinate system. The resulting effect will seem as if you are drawing the spotlight texture at a different location , which is the same as the shader matrix translated location. This is simpler and slighly more efficient

## Summary
- In Android, shader defines the color or the texture with which the Paint object should draw
- Android defines several subclasses of shader for Paint to use, such as BitmapShader, composeShader, linearGradient RadialGradient, sweepGradient
- A Shader defines the content for a paint object which should be drawn. A subclass of shader is installed in a Paint by calling paint.setShader
- Alpha compsoiting is the process of compositing a source image with a destination image to create the appearance of partial or full transparency > The amount of transparency is defined by the alpha channel
- BitmapShader draws a bitmap drawable as a texture. The bitmap can be repeated or mirrored by setting the TileMode mode. 
- The tiling mode, TileMode, defined in the shader, specifies how the bitmap drawable is repeated in teh X and Y direction. Android provides three ways to repeat the bitmap drawble, REPEAT, CLAMP , MIRROR. 

## Motion Events
Object used to report movement evetns. Motions events may hold either absolute or relative movements and other dta, dpeending on the type of device. 

Motion evetns descibe moements in terms of an action code and a set of axis values. The action code specifies the state change that occurred such as a point going down or up . The axis values descibe the position and other movement properties

For example, when the user first touches the screen, the system delivers a touch event to the appropriate view with the action code ACTION_DOWN and a set of axis values that include the X and Y coordinates of the touch and information about the pressure, size and orientation of the contact area. 

Some devices can report multiple movement traces at the same time. Multi-Touch screens emit one movement trace for each finger. The individual fingers or other objects that generate movement traces are referred to as a pointers. Motion events contain information about all of the pointers that are curretnly active even if some of them have not moved since the last event was delivered. 

The number of pointers only ever changes by one as individual pointers go up and down, except when the gesture is canceled. 

Each pointer has a unique id that is assigned when it first goes down(indicated by ACTION_DOWN or ACTION_POINTER_DOWN). A pointer id remains valid until the pointer eventually goes up indicated by ACTION_UP or ACTION_POINTER_UP) OR WHEN THE GESTURE IS CANCELED (indicated by ACTION_CANCEL).

The MotionEvent class provides many methods to query the position and other properties of pointers, such as getX(int), getY(int), getAxisValue(int), getPointerId(int), getToolType(int), and many others. Most of theses methods accept the pointer indiex as a parameter rather than the pointer id. The pointer index of each pointer in the event ranges from 0 to one less than the value returned by getPointerCount()

The order in whichindividual pointers appear within a motion event is undefined. Thus the pointer index of a pointer can change from one event to the next but the pointer id of a pointer is guaranteed to reamin constant as long as the pointer remains active. Use the getPointerId method to obtain the id of a pointer to track it across all subsequent motion events in a gesture. Then for successsive motion events, use the findPointerIndex(int) method to obtain the pointer index for a given pointer id in that motion event. 

Mouse and stylus buttons can be retrieeved using getButtonState(). It is a good idea to check the button state while handling ACTION_WON as part of a touch event. The Application may choose to perform some different action if the touch event starts due to ta secondary button click such as presenting a context menu.


### Batching 
For efficiency, motion events with ACTION_MOVE may batch together multiple movement samples within a single object. The most current pointer coordinates are available using getHistoricalX(int, int) and getHistoricalY(int, int). The coordinates are "historical" only insofar as they are older than the cuurrent coordinates in the batch; however they are still distinct from any other coordinates reported in prior motion events. To process all coordinates in teh batch in time order, first consume the historical coordinates then consume the current coordinates.

### Device Types
The interpretation of the contents of a MotionEvent varies  significantly depending on the source class of the device

On pointing devices with source class InputDevice#Source_CLASSPOINTER such as touch screens, the pinter coordinates specify absolute positions such as view X/Y coordinates. Each complete gesture is represented by a sequence of motion events with actions that descirbe pointer state transistions and movements. A gesture starts with  a motion event with ACTION_DOWN that provides the location of the first pointer down. As each additional pointer that dgoes down or up, the framework will generate a motion event with ACTION_POINTER_DOWN or ACTION_POINTER_UP accordingly. Pointer movements are described by motion events with ACTION_MOVE. Finally, agesture end either when the final pointer goes up as represented by a motion event with ACTION_UP or when gesture is canceled with ACTION_CANCEL.

Some pointe devices such as mice may support vertical and/ horizontal scrolling. A scrolling event is reported as a generic motion event with ACTION_SCROLL that includes the relative scroll offset in the AXIS_VSCROLL and AXIS_HSCROLL axes. See getAsixValue(int) for information about retrieving these additional axes. 

On trackball devices with sources class InputDevice#SOURRCE_CLASS_TRACKBALL, the pointer coordinates specify relative movements as X/Y detlas. Atrackball gesture consists of sequence of movements described by motion events with ACTION_MOVE interspresed with occasional ACTION_DOWN or ACTION_UP motion events when the trackball button is pressed or released.

On joystick devices with source class Input Device#SOURCE_CLASS_JOYSTICK, the pointer coordinates specify the absollute position of the joystick axes. The joystick axis values are normalized to a range of -1 to 1.0 where 0.0 corresponds to the center position. More information about the set of available axes and the range of motion can be obtained using InputDevice#getMotionRange. Some common joystick axes are AXIS_X, AXIS_Y, AXIS_HAT_X,
AXIS_HAT_Y, AXIS_Z and AXIS_RZ. 


### Consistency Guarantees
Motion events are alwasy delivered to views as a consistenet stream of events. What constitutes a consistent stream varies dependingn on the type of device. For touch events, consistency implies that pointers go down one at a time, move around as a group and then go up one at a time or are canceled.

While the framework tries to deliver consistent streams of motion events to views, it cannot guarantee it. Some events may be dropped or modified by containing view in teh application before they are delivered therby making the stream of  events inconsistent. Views should always be prepared to handle ACTION_CANCEL and should tolerate anomalous situations such as receiving a new ACTION_DOWN without first having received an ACTION_UP for the prior gesture. 

