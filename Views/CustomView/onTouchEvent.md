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


example
Move a bitmap
```
override fun onTouchEvent(event: MotionEvent) {
  
  
  when(event.action) {
    MotionEvent.ACTION_DOWN -> {
        // Button pressdown
    }
    MotionEvent.ACTION_MOVE -> {
        // Divide by the bitmap size to keep the pointer centered in the image. 
        bitmapX = event.x = bitmap.width / 2
        bitmapY = event.y = bitmap.height / 2
    }
    MotionEvent.ACTION_UP -> {
    
    }
  }
}
```
