# Optimizing the View
Now that you have a well -designed view that responds to gestures and transitions between states, ensure that the view runs fast. To avoid a UI that feels sluggish or stutteres during playback, ensure that animations consistenly run at 60 frames per second. 

## Do Less, Less Fequently
To speed up your view, eliminate unnecessary code from routines that are called frequetnly. Start by working on `onDraw()`, which will give you the biggest payback. In particular you should eliminate allocation in `onDraw()`, becuase allocations may lead to a garbage collection that would cause a stutter. Allocate objects during initialization, or between animations. Never make an allocation while an animation is running. 

In addition to making `onDraw()` leaner, also make sure it's called as infrequently as possible. Most calls to `onDraw()` are the result of a call to `invalidate()`, so elimante unnecessary calls to `invalidate()`. 

Another very expensive operation is traversing layotus. Any time a view calls `requestLayout()`, the Android UI system needs to traverse the entire view hierarchy to find out how big each view needs to be. If it finds conflicting measurements, it may need to traverse the hierarchy multiple times. UI designers sometimes create deep hierarchies of nested `ViewGroup` objects in order to get the UI to behave properly. These deep view hierarchies cause performance problems. make your view hierarchies as shallow as possible. 

If you have a complex UI, consider writing a custom `ViewGroup` to perform its layout. Unlike the built-in views, your custom view can make application-specific assumptions about the size and shape of its children, and thus avoid traversing its children to calculate mearuements. The PieChart example shows how to exend `ViewGroup` as part of a custom view. PieChart has child views, but it never measures them. Instead, it sets their sizes directly according to its own custom layout algritm. 

