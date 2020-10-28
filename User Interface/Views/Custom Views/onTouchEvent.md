# OnTouchEvent

Implmenet this method ot handle touch screen motion events. If this method is used to detect click actions, it is recommended that the actions be performed by implmenenting the calling performClick(). This will ensure consistent system bhavior, including
- Obeying click sound preferences
- dispactingi9n onClickListeners calls
- handling AcessibilityNodeInfo#ACTION_CLICK when accessibility features are enbabled

Notes 
When onTouchEvent() is called it creates a motionEvent for you, if the event is succesful return true. 


Example
```
override fun onTouchEvent(event: MotionEvent): Boolean {

}
```

You can use the following .do notation on the event to preform action
- ACTION_DOWN:  When pressed this is action will fire. 
- ACTION_MOVE: A change has happened during a press gesture Between ACTION_DOWN and ACTION_UP. The motion contains the most recent point as well as any intermediate points since the last down or move event. 
- ACTION_UP: This event occurs when the finger is lifted from the screen. 


## example
### Move a bitmap
```
override fun onTouchEvent(event: MotionEvent) {
  
  
  when(event.action) {
    MotionEvent.ACTION_DOWN -> {
        // Canvas pressed pressdown
    }
    MotionEvent.ACTION_MOVE -> {
        // Divide by the bitmap size to keep the pointer centered in the image. 
        bitmapX = event.x = bitmap.width / 2
        bitmapY = event.y = bitmap.height / 2
    }
    MotionEvent.ACTION_UP -> {
      // Canvas resleased. 
    }
  }
  // Run the onDraw()
  invalidate()
 
  // If action is preformed return true
  return true
}
```
### Move bitmap one or two
```

var bitmapOneX
var bitmapOneY
var bitmapTwoX
var bitmapTwoY

var bitmapOneBoo = false
var bitmapTwoBoo = false

override onTouchEvent(event: MotionEvent) {
  when(event.action) {
  MotionEvent.ACTION_DOWN -> {
    // Set up the values for bitmapOne click zone. 
    val bitmapOneLeft = bitmapOneX
    val bitmapOneRight = bitmapOneX + bitmapOne.width
    val bitmapOneTop = bitmapOneY
    val bitmapOneBottom = bitmapOneY + bitmapOne.Height
    if(bitmapOneLeft < event.x &&
      bitmapOneRight > event.x &&
      bitmapOneTop < event.y &&
      bitmapOneBottom > event.y
    ) {
      bitmapOneBoo = true
   }
    
    val bitmapTwoLeft = bitmapTwoX
    val bitmapTwoRight = bitmapTwoX + bitmapTwo.width
    val bitmapTwoTop = bitmapTwoY
    val bitmapTwoBottom = bitmapTwoY + bitmapTwo.height
    
    if(bitmapTwoLeft < event.x &&
      bitmapTwoRight > event.x &&
      bitmapTwoTop < event.y &&
      bitmapTwoBottom < event.y
    ) {
      bitmapTwoBoo = true
    }
    
  }
  MotionEvent.ACTION_MOVE -> {
    if(bitmapOneBoo) {
      bitmapOneX = event.x - bitmapOne.width / 2
      bitmapOneY = event.y - bitmapOne.height / 2
    }
    
    if(bitmapTwoBoo) {
      bitmapTwoX = event.x - bitmapTwo.width / 2
      bitmapTwoY = event.y - bitmapTwo.height / 2
    }
  }
  MotionEvent.ACTION_UP -> {
    bitmapOneBoo = false
    bitmapTwoBoo = false
  }
  
  invalidate()
  return true
}
```
