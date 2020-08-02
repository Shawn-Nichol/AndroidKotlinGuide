# Motion Events

Object used to report movement events. Motions events may hold either absolute or relative movements and other data, depending on the type of device. 

Motion events descibe moments in terems of an action code and a set of axis values. The ation code specifies the state changed that occurred such as a point going down or up. The axis values descibe the position and other movement properties. 

For example, when the user first touches the screen, the system delivers a touch event to the appropriate view with the actioncode ACTION_DOWN and a set of axis values that include the X and Y coordinates of the touch and information about the pressure, size and orientation of the contact area. 

Some devices can report multiple movement traces at the same time. Multi-Touch screens emit one movement trace for each finger. The individual fingers or the other objects that generate movement traces are referred to as a pointers. Motion events contain information about all of the pointers that are curretnly active even if some of them have not moved since the last event wasa delivered. 

Teh number of pointers only ever changes by one as individual pointers go up and down, except when the gesture is canceled. 

Each pointer has a unique id that is assigned when it first goes down(indicated by ACTION_DOWN or ACTION_POINTER_DOWN). A pointer ID remains valid until the pointer eventually goes up indicated by ACTION_UP or ACTION_POINTER_UP) or when the gesture is canceled(ACTION_CANCEL).

The MotionEvent class provides many methods to query the position an dother properties of pointers, such as getX(int), getY(int), getAxisValue(int), getPointerId(int), getToolType(int, and many others. Most of these methods accept the pointer index as a parameter rather than the pointer id. The pointter index of each pointer in the event ranges from 0 to one less than the value returned by getPointerCount()

The order in which individual pointers appear within a motion event is undefined. Thus the pointer index of a pointer can change from one method to obtain the id of a pointer to track it across all susequent motion evetns in a gesture. Then for successive motionevents, use the findPointerIndex(int) method to obtain the pointer index for a given pointer id in that motion event. 

Mouse and stylus buttons can be retrieved using getButtonState(). It is a good idea to check the button state while handling ACTOIIN_WON as part of a touch event. The application may choose to perform some different action if the touch event starts due to the secondary button click such as presenting a context menu. 

## Batching
For efficiency, motion events with ACTION_MOVE may batch toegther multiple movements amples within a single object. The most current pointer coordinates are available using getHistoricalX(int, int0 and getHistroicalY(int, int). The coordinates are "historical" only insofar as they are older than the current coordinates in the batch; however they are still distinct from any other coordinates reported in prior motion events. To process all coordinates in the batch in time order, first consume the histroical coordinates then consume corrdiantes

Device Types
The interpretation of the contents of a MotionEvent varies significantly depending on the source class of the device

On pointing devices with source class InputDevice#Source_CLASSPOINTER such as touch screens, the pinter coordinates specify absolute positions such as view X/Y coordinates. Each complete gesture is represented by a sequence of motion events with actions that descirbe pointer state transistions and movements. A gesture starts with a motion event with ACTION_DOWN that provides the location of the first pointer down. As each additional pointer that dgoes down or up, the framework will generate a motion event with ACTION_POINTER_DOWN or ACTION_POINTER_UP accordingly. Pointer movements are described by motion events with ACTION_MOVE. Finally, agesture end either when the final pointer goes up as represented by a motion event with ACTION_UP or when gesture is canceled with ACTION_CANCEL.

Some pointe devices such as mice may support vertical and/ horizontal scrolling. A scrolling event is reported as a generic motion event with ACTION_SCROLL that includes the relative scroll offset in the AXIS_VSCROLL and AXIS_HSCROLL axes. See getAsixValue(int) for information about retrieving these additional axes.

On trackball devices with sources class InputDevice#SOURRCE_CLASS_TRACKBALL, the pointer coordinates specify relative movements as X/Y detlas. Atrackball gesture consists of sequence of movements described by motion events with ACTION_MOVE interspresed with occasional ACTION_DOWN or ACTION_UP motion events when the trackball button is pressed or released.

On joystick devices with source class Input Device#SOURCE_CLASS_JOYSTICK, the pointer coordinates specify the absollute position of the joystick axes. The joystick axis values are normalized to a range of -1 to 1.0 where 0.0 corresponds to the center position. More information about the set of available axes and the range of motion can be obtained using InputDevice#getMotionRange. Some common joystick axes are AXIS_X, AXIS_Y, AXIS_HAT_X, AXIS_HAT_Y, AXIS_Z and AXIS_RZ.

Consistency Guarantees
Motion events are alwasy delivered to views as a consistenet stream of events. What constitutes a consistent stream varies dependingn on the type of device. For touch events, consistency implies that pointers go down one at a time, move around as a group and then go up one at a time or are canceled.

While the framework tries to deliver consistent streams of motion events to views, it cannot guarantee it. Some events may be dropped or modified by containing view in teh application before they are delivered therby making the stream of events inconsistent. Views should always be prepared to handle ACTION_CANCEL and should tolerate anomalous situations such as receiving a new ACTION_DOWN without first having received an ACTION_UP for the prior gesture.
