# View
This class represents the basic building block for the UI components. A view occupies a rectangular area on the screen and is responsible for drawing and event handling. The View is the base class for widgets which are used to create interactive UI components (Buttons, Text, etc) The ViewGroup subclass is the base class for layouts, which are invisible containers that hold other Views and define their layout properties. 

## Using View
All of the views in a window are arranged in a single tree. You can add views either from code or by specifying a tree of views in one or more XL layout files. There
are many specialized subclasses of views that act as controls or are capable of displaying text, images, or other content. 

Common operations 
- Set Properties: Ex setting the text of a TextView, Properties that are none at build time can be set in the XML layout.
- Set focus: The framework will handle moving focus in response to user input. To force focus to a specific view, call requestFocus()
- Set up listeners: Views all clients to set listeners that will be notified when something interesting happens to the view. 
- Set visibility: You can hide or show a view. 

## IDs
Views may have an integer id associated with them. These ids are typically assigned in the layout XML files and are used to find specific views 
within the view tree.
Define a button in the layout and assign it a unique ID, from the onCreate method of an Activity, find the button. 

## Position
The geometry of a view is that of a rectangle. A View has a location, expressed as a pair of left and top coordinates, and two dimensions expressed as a width and a height. The unit for location and dimensions is the pixel

It is possible to retrieve the location of a view by invoking the methods getLeft() and getTop(). The former returns the left, or X coordinate of the rectangle representing the view. The latter returns the top or Y coordinate of the rectangle representing the view. These methods both return the location of the view relative to its parent. For instance, when getLeft9) returns 20 that means the view is located 20 pixels to the right of the left edge of its direct parent. 

In addtion several convenience methods are offered to avoid unnecessary computations, namely getRight() and getBottom(). These methods return the coordinates of the right and bottom edges of the rectangle representing the view. For instance, calling getRight9) is similar to the following computation: getLeft() + getWidth()

## Size padding and margins
The size of a view is express with a width and height. A view actually possesses two pairs of width and height values. 

The first pair is known as measured width and measured height. These dimensions define how big a view wants to be within its parent. The measured dimensions can be 
obtained by calling getMeasuredWidth() and getMeasuredHeight()

The second pair is simply known as width and height, or sometimes drawing width and drawing height. These dimensions define the actual size of the view on screen, at drawing time and after layout. These values may, but do not have to, be different from the measured width and height. The width and height can be obtained by calling getWidth() and getHeight()

To Measure its dimensions, a view takes into account it's padding. The padding is expressed in pixels for the left, top, right, and bottom parts of the view. Padding can be used to offset the content of the view by a specific amount of pixels. For instance, a left padding of 2 will push the view's content by 2 pixels to the right of the left edge. padding can be set using the setPadding() or setPaddingREaltive method and queried by calling 

Even though a view can define a padding, it does not provide any support for margins. However, view groups provide such support. Refer to a ViewGroup and ViewGroup.MarginLayoutParams for further information

## Layouts
The layout is a two-pass process: a measure pass and a layout pass. The measuring pass is implemented in measure(int, int) and is a top-down traversal of the view Tree. Each view pushes dimension specifications down the tree during the recursion. At the end of the measure pass, every view has stored its measurements. The second pass is responsible for the position in the layout(int, int, int, int) and is also top-down. During this pass, each parent is responsible for positioning all of its children using the sizes computed in the measure pass. 

When a view's measure() method returns, its getMeasuredWidth() and getMeasureHieght() values must be set, along with those for all of that view' descendants. A view's measured width and measured height values must respect the constraint imposed by the view's parents. This guarantees that at the end of the measure pass, all parents accept all of their children's measurements. A parent view may call measure() more than once on its children. For example, the parent may measure each child once with unspecified dimensions to find out how big they want to unconstrained sizes is too big or too small

The measure pass uses two classes to communicate dimensions. The measureSpec class is used by views to tell their parents how they want to be measured and positioned the base layout params class just describes how big the view wants to be for both width and height. For each dimension, it can specify one of
  - Exact number
  - MATCH_PARENT, which means the view wants to be as big as its parent
  - WRAP_CONTENT, the view wants to be as big it's content. 
  
There are subclasses of LayoutPrams for different subclasses of ViewGroup. For example, AbsoluteLayout has its own subclass of LayoutParams which adds an X and Y value. 

MeasureSpecs are used to push requirements down the tree from parent to child. A MeasureSpec can be in one of three modes
  - UNSPECIFIED: This is used by a parent to determine the desired dimension of a child view
  - EXACTLY:  This is used by the parent to impose an exact size on the child. The child must use this size and guarantee that all of its descendants will fit within this size. 
  - AT_MOST: This is used by the parent to impose a maximum size on the child. The child must guarantee that it and all of its descendants will fit within this size. 
  
  To initiate a layout, call requestLayout(). This method is typically called by a view on itself when it believes that it can no longer fit within its current bounds. 
  
# Drawing 
Drawing is handled by walking three and recording the drawing commands of any View that needs to update. After this, the drawing commands of the entire tree are issued to screen, clipped to the newly damaged area. 

The tree is largely recorded and dawn in order, with parents drawn before their children, with siblings drawn in the order they appear in the tree. If you set a background drawable for a View, then the View will draw it before calling back to its onDraw() methods. The child drawing order can be overridden within a ViewGroup and with sets custom Z values set on Views

To force a view to draw call invalidate()

## Event Handling and Threading
The basic cycle of a view is as follows
1) An event comes in and is dispatched to the appropriate view. The view handles the event and notifies any listeners
2) If in the course of processing the event, the view's bounds may need to be changed, the view will call requestLayout()
3) Similarly if in the course of processing the event the view's appearance may need to be changed, the view will call invalidate()
4) If either requestLayout() or invalidate() were called, the framework will take care of measuring, layout, and drawing the trees as appropriate. 

Note: The entire view tree is single-threaded. You must always be on the UI thread when calling any method on any view. If you are doing work on other threads and want to update the state of a view from that thread, you should use a Handler. 

## Focus Handling
The framework will handle routine focus movement in response to user input. This includes changing the focus as views are removed or hidden, or as new views become available. Views indicate their willingness to take focus through the isFocusable() methods. To change whether a view can take focus, call setFocusable. When touch mode views indicate whether they still would like focus via is FocusableInTouchMode() and can change this via setFocusableInTouchMode

Focus movement is based on an algorithm which finds the nearest neighbor in a given direction in rare cases, the default algorithm may not match the intended behavior of the developer. In these situations, you can provide explicit overrides by using theses XML attributes in the layout file. 

To get a particular view to take focus call requestFocus()

## Touch Mode

When a user is navigating a user interface via directional keys such as a D-pad, it is necessary to give focus to actionable items such as buttons so the user can see what will take input. If the device has touch capabilities, however, and the user begins interacting with the interface by touching it, it is no longer necessary to always highlight, or give focus to, a particular view. This motivates a mode of interacting named 'touch mode'

For a touch-capable device, once the user touches the screen, the device will enter touch mode. From this point onward, only views for which is focusable INtouchmode is true will be focusable such as text editing widgets. Other views that are touchable, like buttons, will not take focus when touched they will only fire the on click listeners. 

Any time a user hits a directional key, such as a D-pad direction, the view device will exit touch mode, and find a view to take focus, so that the user may resume interacting with the user interface without touching the screen again. 

The touch mode state is maintained across Activities. Call isIntouchMode to see whether the device is currently in touch mode. 

## Scrolling 
The framework provides basic support for views that wish to internally scroll their content. This includes keeping track of the X and Y scroll offset as well as mechanisms for drawing scrollbars.

## Tags
Unlike IDs, tags are not used to identify views. Tags are essentially an extra piece of information that can be associated with a view. They are most often used as a convenience to store data related to views in the views themselves rather than by putting them in a separate structure. 

Tags may be specified with character sequence values in layout XML as either a single tag using the android: tag attribute or multiple tags using the <tag> child element
  
Tages may also be specified with arbitrary objects from code using setTag()

## Themes

By default, Views are created using the theme of the Context object supplied to their constructor; however, a different theme may be specified by using the android: theme attribute in layout XML or by passing a ContextThemeWrapper to the constructor from code. 

When the android: theme attribute is used in XML, the specified theme is applied on top of the inflation context theme and used for the view itself as well as any child elements 

## Properties
The View class exposes an ALPHA property, as well as several transform-related properties, such as TRASLATION_X and TRANSLATION_Y. These properties are available both in the Property form as well as in similarly-named setter/getter methods. These properties can be used to set persistent state associated with these rendering -related properties on the view. The properties an methods can also be used in  conjunction with Animator-based animations, described more in the Animation section

## Animation
Starting with Android 3.0, the preferred way of animating views is to use the android.animation package APIs. These Animator-based classes change the actual properties of the View object, such as alpha and translationX. This behavior is contrasted to that of the pre-3 Animation-based classes, which instead animate only how the view is drawn on the display. IN particular, theViewPRoertyAnimator class makes animating theses View properties particularly easy and efficient. 

## Security
Sometimes it is essential that an application be able to verify that an action is being performed with the full knowledge and consent of the user, such as granting a permission request, making a purchase, or clicking on an advertisement. Unfortunately, a malicious application could try to spoof the user into performing these actions, unaware, by concealing the intended purpose of the view. As a remedy, the framework offers a touch filtering mechanism that can be used to improve the security of views that provide access to sensitive functionality. 

To enable touch filtering, call setFilterTouches WhenObscured layout
  










## How Android draws Views
When an Android activity comes into the foreground, Android asks it for its root view. The root view is the top parent of the layout hierarchy. Android then starts drawing the whole view hierarchy. 

Android draws the hierarchy starting from the top parent, then its children,  and if one of the children also a ViewGroup, Android will draw its children before drawing the second child. So it's a depth-first traversal. 

Android draws the children of a ViewGroup according to the index of the child(its position in the XML file).

Android draws the layout hierarchy in three stages
1) Measuring stage: each view must measure itself.
2) Layout stage: each Viewgroup finds the right position for its children on the screen by using the child size and also by following the layout rules. 
3) Drawing sage after measuring and positioning all of the views, each view happily draws itself

