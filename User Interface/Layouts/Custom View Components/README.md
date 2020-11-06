### View
This class represents the basic building block for user interface components. A view occupies a rectanglar area on the screen and is responsible for drawing and event handling. 

- `onDraw(canvas: Canvas)`: 
Does your drawing
    - Canvas: The canvas on which the background will be drawn on. 

- `onMeasure(widthMeasureSpec, heightMeasureSpec): 
Measure the view and its content to determine the measured width and the measured height. 
  - widthMeasureSpec: Horizontal space requirements as imposed by the apretn. 
  - heightMeasureSpec: int: vertical space requiremtnets as imposed by the parent. 
  The requiremetns are encoded with View.MeasureSpec

- `invalidate()`:
Invalidate the whole view. If the view is visible, `onDraw(android.graphics.Canvas)` will be called at some pointin the future. This must be called form a UI thraead. To call from a non UI thread call `postInvalidate()`

- `postInvalidate`:
Cause an invalidate to happen on a subsequent cycle through the event loop. use this to invalidate the View from a non-UI thread. This method can be invoked from outside of the UI thraed only when this View is attached to a window. 

- `requestLayout()`:
Call this when someting has changed which has invalidated the layout of this view. THis will schedule a layout pass of the view tree. This should not be called while the view hierarchy is currently in a layout pass (isLayout()). If layout is happening, the request may be honored at the end of the current layout pass (and then layout will run agina0 or after the current frame is drawn and the next layout occurs. 



### View.MeasureSpec
A MeasureSPec encapsulates the layout requirement passed form parent to child. Each MeasureSPec represnts a reuirement for either the width or the height. A MeasureSpec is commprised of a size and a mode. There are three possible modes. 
1. Unspecified: the parent has not imposed any constraint on the child. It can be whatever size it wants. 
2. Exactly: The parent has determined asn exact size for the child. The child is  going to be given those bounds regardless of how bit it wants to be. 
3. AT_MOST: The child can be as large as it wants up the specified  size. 

MeasuredSPecs are implemented as ints to reduce object allocation. This class is provided to pack and unpack the <size, mode. tuple into the int. 

### Canvas
The Canvas class hold the "draw" calls. To draw something, you need 4 basic components: A bitmap to hold the pixels, a Canvas to host the draw calls(writing into the bitmap), a drawing primitive(eg.Rect, Path, Text, Bitmap), and paint (to descirbe the colors and styles for the drawing). 

### Paint
The paint clas sholds the stle and color informationa bot how to draw geometries, text and bitmpas. 

### Path
The path class encapsulates compound (multiple contour) geometric paths consisting of straight line segments, quadrdatic curves, and cubic curves. It can be drawn with canvas.drawPath(path paintn), either filled or storked based on the paints style or it can be used for clipping or to draw text on a path. 

## GestureDetector
Detects various gestures and events using the supplied `MotionEvents`. The `OnGestureListener` callback will notify users when a particular motion event has occurred. This class should only be used with `MotionEvent`s reported via touch (don't use for trackball events). 


### Scroller
This class enapsulates scrolling. You can use scrollers (Scroller or OVerScroller) to collect the data you need to produce a scrolling animation. Scrollers track scroll offsets for you over time, but they don't automtaically apply those positions ot your view. 

- `fling`: Start scrolling based on a fling gesture. The distance travelled will dpened on the initial velocity ofthe fling. 

### Value Animator
This class provides a simple timging engine for running amimations which calculate animated values and set them on target objects. There is a single timing pulse that all animations use. It runs in a custom handler to ensure that property changes happen on the UI thread. 

### AttributeSet
A collection of attributes, as foudn associated with a tag in XML document. Often you will not want to use this interface directly, instead passing it to `Resource.Theme.obtainStyledAttributes()` which will take care of parsing the attributes for you. In particular, the Resources API will convert resource references (attribute values such as "@String/my_label"in the oringal XML) to the desired type for you; if you use AttributeSet driectly then you will need tomanually check for resource references and do the resource looup yourself  if needed. Directuse of AttributeSet also prevents the application of themse and styles when retrieving attributte values. 

This interface providesw an efficient mchanism for retireving data from copmiled XML files, which can be retrieved for a particular XmlPullParser through XML#asAttributeSet. Nomrally this will return an implementation of theinterface that works on top of a generic XmlPullParser, however it is mreo useful in conjunction with compiled XML resources. 
