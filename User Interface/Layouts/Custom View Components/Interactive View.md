# Making the View Interactive
Drawing a UI is only one part of creating a custom view. you also need to make your view respond to user input in a way  tht closely  reembles the real-world action you're mimikcing. Ojbects should always act in the same way that real objects do. For example, images should not immediatelly pop out of existence and reappear somewhere else, becuase objects in the real workd don't do that. Instead, images should move from one place to another. 

Users also sense subtle behavior or feel in an interface, and react best to subtleties that mimic the real workd. For example, when users fling a UI object, they should sense friction at the geinning that delays the motion, and then at the end sense momentum that carries the motion beyond the fling. 

This leeson demonstrates how to use features of the Android framework to add these real-world behaviors to your custom view. 

Beyond this lesson you can find additiona l realted information at `Input Events` and `Property Animation`. 

## Handle Input Gestures
Like many other UI frameworks, Android supports an input event model. User actions are turnedinto events that trigger callbacks, and you can override the callbacks to customize how your application responds to the user The most common input event in the Android system is touch, which triggers `onTouchEvent(android.view.MotionEvent)`. Override this method to handle the event. 

```
override fun onTouchEvent(event: MotionEvent): Boolean {
    return super.onTouchEvent(event)
}
```
Touch events by themselves are snot particularly useful. Modern Touch UIs define interactsion in temers of gestures such as tapping, pulling, pusing, flinging, and zooming. To onvert raw touch events into gestures, Android provides `GestureDetector`.

Construct a `GestureDectectore` by passing in  an instance of a class that implements `GestureDectector.OnFestureListener`. If you only want to process a few gestures, you can extend `GestureDetector.SimpleOnGestureListener` instead of implementing the `FestureDetector.OnFestureListener` interface .For instance, this code creates a class thate xtends `GestureDectore.SimpleOnFestureListener and overrides `onDown(MotionEvent)`. 

```
private val myListener =  object : GestureDetector.SimpleOnGestureListener() {
    override fun onDown(e: MotionEvent): Boolean {
        return true
    }
}

private val detector: GestureDetector = GestureDetector(context, myListener)
```

Whether or not you use `GestureDetector.SimpleOnFestureListener`, you must always implement an `onDown()` method that returns true. This step is necessary because all festures begin with an `onDown()` message. If youreturn false from `onDown()`, as `GestureDetector.SimpleOnFestureListener` does, the system assumes that you want to ignore the rest of the gesture, and the other methods of `FestureDetector.OnGestureListener never get called. The onlly time you should return false from `onDown() is if you truly want to ignore an entire gesture. Once you've impelmented `FestureDectectore.OnFestureListener and created an instance of `GestureDetector`, you can use your `FestureDetector to interpret the touch events you recieve in onTouchEvvent(). 

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
Gestures are a powerful way to control touchscreen devices, but they can be counterintutive and difficult to remember unless they produce physicallly plausible results. A good example of this is the fling gesture, where the user quickly moves a finger across the screen and then lifts it. This festure makes sense if the UI responds by moving quickly in teh direction of the fling, then slowing down, as if the user had pushed on a flywhelel and set it spinning. 

However, simulating the feel of a flywheel isn't trival. A lot of physics and math are required to get a flywheel mdoel woriing correclty. Fortunatley, Android provides helper classes to simulate this andd other behaviors. The `Scroller` class is basis for handling flywheel-style fling gestures. 

To start a fling, call `fling()` with the starting velocity and the mimimum and maximum x and y values of the fling. For the velocity value, you can use the value computed for you by `GestureDetector. 

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

Note although the velocity calculated by GestureDetector is physically accurate, many developers feel that using this valu emakes the fling animation too fast. It's common to divide the x and y velocity by a factor of 4 to 8

The call to `fling()` sets up the physics model for the fling gesture. Afterwards, you need to update the `Scroller` by calling `Scroller.computeScrollOffset()` at regular intervals `computeScrollOffset()` updates the `Scroller` object's internal state by reading the current time and using th ephysics model to calculate the x and y position at that time. Call `getCurrX()` and `getCurrY()` to retrieve these values. 

most views pass the `Scroller` object's x and y position directly to `scrollTo()`. The PieChart example is a little different it uses the current scrolly position to set the rotational angle of the chart. 

```
scroller.apply {
    if (!isFinished) {
        computeScrollOffset()
        setPieRotation(currY)
    }
}
```

The `Scroller class computes scroll positions for you, but it does not autmatically apply those positions to your view. It's your esponsibilit ot make sure you get and apply new coordinates often enought to make the scrolling animation look smooth. THere are two ways to do this. 
1. Call `postInvalidate()` after calling `fling()`, in order to force a redraw. This technique requires that you compute scroll offsets in `onDraw()` and call `postInvalidate()` every time the scroll offset changes. 
2. Set up a `ValueAnimator` to animate for the duration of the fling, and add a listener to process animation updates by calling `addUpdateListener()`. 

The PieChart example uses the second approach. This techinque is slightly more complex to set up, but it works more closely with animation system and doesn't require potentially unnecessary view invalidation. The drawback is that `ValueAnimator` is not aviablel prior to API level 11, so this techinque cannot be used on devices running Android version lower than 3.0

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
Users expect a modern UI to transition smoothly between states. UI elements fade in and out instead of appearing and disappearing. Motions begin and ends smoothly instead of starting and stopping abruptly. The android property animation framework, tinroduced in Android 3.0 makes smooth transitions easy. 

To use the animation system, whenever a property changes that will affect your view's appearance, do not change the property directly. Instead, use `ValueAnimator` to make the change. Inteh following example, modifying the currently selected pie slice in Pie Chart causes the entire chart to rotate so that the selection pointer is centered in teh selected slice. ValueAnimator changes the rotation over a period of several hundred milliseconds, rather than immediately setting the new rotation value. 

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
