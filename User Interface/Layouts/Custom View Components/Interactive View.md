# Making the View Interactive
Ojbects should always act in the same way that real objects do. For example, images should not immediatelly pop out of existence and reappear somewhere else, becuase objects in the real work don't do that. Instead, images should move from one place to another. 

Users also sense behavior or feel in an interface, and mimic the real workd. For example, when users fling a UI object, they should sense friction at the begining that delays the motion, and then at the end sense momentum that carries the motion beyond the fling. 

## Handle Input Gestures
Like many other UI frameworks, Android supports an input event model. User actions are turned into events that trigger callbacks, and you can override the callbacks to customize how your application responds to the user. The most common input event in the Android system is touch, which triggers `onTouchEvent(android.view.MotionEvent)`. Override this method to handle the event. 

```
override fun onTouchEvent(event: MotionEvent): Boolean {
    return super.onTouchEvent(event)
}
```
Touch events by themselves are not particularly useful. Modern Touch UIs define interactsion in terms of gestures such as tapping, pulling, pusing, flinging, and zooming. To convert raw touch events into gestures, Android provides `GestureDetector`.

Construct a `GestureDectectore` by passing in an instance of a class that implements `GestureDectector.OnGestureListener`. If you only want to process a few gestures, you can extend `GestureDetector.SimpleOnGestureListener` instead of implementing the `GestureDetector.OnGestureListener` interface. For instance, this code creates a class that extends `GestureDectore.SimpleOnGestureListener and overrides `onDown(MotionEvent)`. 

```
private val myListener =  object : GestureDetector.SimpleOnGestureListener() {
    override fun onDown(e: MotionEvent): Boolean {
        return true
    }
}

private val detector: GestureDetector = GestureDetector(context, myListener)
```

Whether or not you use `GestureDetector.SimpleOnFestureListener`, you must always implement an `onDown()` method that returns true. This step is necessary because all festures begin with an `onDown()` message. If you return false from `onDown()`, as `GestureDetector.SimpleOnFestureListener` does, the system assumes that you want to ignore the rest of the gesture, and the other methods of `GestureDetector.OnGestureListener` never get called. The only time you should return false from `onDown()` is if you truly want to ignore an entire gesture. Once you've impelmented `GestureDectectore.OnGestureListener` and created an instance of `GestureDetector`, you can use your `GestureDetector` to interpret the touch events you recieve in onTouchEvvent(). 

```
override fun onTouchEvent(event: MotionEvent): Boolean {
    return detector.onTouchEvent(event).let { result ->
        if (!result) {
            if (event.action == MotionEvent.ACTION_UP) {
                stopScrolling()
                true
            } else false
        } else true
    }
}
```

When you pass `onTouchEvent()` a touch event that it doesn't recongize as part of a gesture, it returns false. You can then run your own custom gesture-detection code. 

## Create Physically Plausible Motion
Gestures are a powerful way to control touchscreen devices, but they can be counter intutive and difficult to remember unless they produce physicallly plausible results. A good example of this is the fling gesture, where the user quickly moves a finger across the screen and then lifts it. This gesture makes sense if the UI responds by moving quickly in the direction of the fling, then slowing down, as if the user had pushed on a flywheel and sent it spinning. 

However, simulating the feel of a flywheel isn't trival. A lot of physics and math are required to get a flywheel model working correclty. Fortunatley, Android provides helper classes to simulate this and other behaviors. The `Scroller` class is basis for handling flywheel-style fling gestures. 

To start a fling, call `fling()` with the starting velocity and the mimimum and maximum x and y values of the fling. For the velocity value, you can use the value computed for you by `GestureDetector`. 

```
fun onFling(e1: MotionEvent, e2: MotionEvent, velocityX: Float, velocityY: Float): Boolean {
    scroller.fling(
            currentX,
            currentY,
            (velocityX / SCALE).toInt(),
            (velocityY / SCALE).toInt(),
            minX,
            minY,
            maxX,
            maxY
    )
    postInvalidate()
    return true
}
```

Note: although the velocity calculated by GestureDetector is physically accurate, many developers feel that using this value makes the fling animation too fast. It's common to divide the x and y velocity by a factor of 4 to 8

The call to `fling()` sets up the physics model for the fling gesture. Afterwards, you need to update the `Scroller` by calling `Scroller.computeScrollOffset()` at regular intervals `computeScrollOffset()` updates the `Scroller` object's internal state by reading the current time and using the physics model to calculate the x and y position at that time. Call `getCurrX()` and `getCurrY()` to retrieve these values. 

most views pass the `Scroller` object's x and y position directly to `scrollTo()`. The PieChart example is a little different it uses the current scrolly position to set the rotational angle of the chart. 

```
scroller.apply {
    if (!isFinished) {
        computeScrollOffset()
        setPieRotation(currY)
    }
}
```

The `Scroller` class computes scroll positions for you, but it does not autmatically apply those positions to your view. It's your responsibility to make sure you get and apply new coordinates often enough to make the scrolling animation look smooth. There are two ways to do this. 

1. Call `postInvalidate()` after calling `fling()`, in order to force a redraw. This technique requires that you compute scroll offsets in `onDraw()` and call `postInvalidate()` every time the scroll offset changes. 
2. Set up a `ValueAnimator` to animate for the duration of the fling, and add a listener to process animation updates by calling `addUpdateListener()`. 

Note: You can use the ValueAnimator in applications that target lower API levels. You just need to make sure to check the current API level at runtime, and omit the calls to view animation system if the curretn level is less than 11. 

```
private val scroller = Scroller(context, null, true)
private val scrollAnimator = ValueAnimator.ofFloat(0f, 1f).apply {
    addUpdateListener {
        if (scroller.isFinished) {
            scroller.computeScrollOffset()
            setPieRotation(scroller.currY)
        } else {
            cancel()
            onScrollFinished()
        }
    }
}
```

## Make your transitions smooth
Users expect a modern UI to transition smoothly between states. UI elements fade in and out instead of appearing and disappearing. Motions begin and ends smoothly instead of starting and stopping abruptly. 

To use the animation system, whenever a property changes that will affect your view's appearance, do not change the property directly. Instead, use `ValueAnimator` to make the change. In the following example, modifying the currently selected pie slice in Pie Chart causes the entire chart to rotate so that the selection pointer is centered in teh selected slice. ValueAnimator changes the rotation over a period of several hundred milliseconds, rather than immediately setting the new rotation value. 

```
autoCenterAnimator = ObjectAnimator.ofInt(this, "PieRotation", 0).apply {
    setIntValues(targetAngle)
    duration = AUTOCENTER_ANIM_DURATION
    start()
}
```

If the vlue you want to change is one of the base `View` properites, doing the animation is even easier, becuase Views have a built-in `ViewPropertyAnimtor` that is optimized for simultaneous animation of multiple properties. 
```
animate()
    .rotation(targetAngle)
    .duration = ANIM_DURATION
    .start()
```
