# Input events Overview
On Android, there's more than one way to intercept the events froma user's interaction with your application. When considering events within your user interface, the approach is to capture the events from the specific View object taht the user interacts with. The View class provides the means to do so. 

Within the various view classes that you'll use to compose your layout you may notice several public callback methods that look useful for UI evetns. These methods are called by Android framework when the respective action occurs on that object. For instance, when a View is touched, the onTouchEventmethod is called on that object. However, in order to intercept this, you must extend the class and override the method. However, extending every View object in order to handle such as event would not be practical. This is why the View class also contains a collection of nested interfaces with callbacks that you can much more easily define. THese interfaces, called event listeners, are your ticket to capturing the user interaction with your UI. 

While you will more commonly use the event listeners to listen for user interaction, there may come a  time when you do want to extend a View class, in order to build a custom component. Perhaps you want to extend the button class to make somethign more fancy. In this case you'll be able to define the default  event behaviors for your class using the class event handlers. 

## Event Listeners
An event listener is an interface in the View class that contains a single callback method. These methods will be called by the Android framework when the View to which the listener has been registered is triggered by user interaction with the item in the UI. 

Included in the event listener interfacese are the following callbacks

### onClick()
From  View.ClickListener. This is called when the user either touches the item(when in touch mode), or focuses upon the item with the navigation-keys or trackball and presses the suitable "enter" key or presses down on the thr trackball. 

### onLongClick()
From View.OnLongClickListener. This is called when the user either touches and holds the item(when in touch mode), or focuses upon the item with the navigation-keys or trackball and presses and holds the suitable enter key or presses and hold down on the trackball for one second. 

### onFocusChange()
From View.OnFocusChanngeListener. THis is called when the user navigates onto or away from the item, using the navigation-keys or trackba..

### onKey()
From View.onKeyListener. This is called when the user is focused on the item and presses or releases a hardware key on the device

### onTouch()
From View..OnTouchListener. This is called when the user performs an action qualified as a touch event, including a press, a release, or any movement ggesture on the screen(within the bounds of the item)

### onCreateContextMenu()
From View.onCreateContextMenuListener. THis is called when a context Menu is being built(as the result of a sustained "long click").

These methods are the sole inhabitants of their respective interface. To define one of these methdos and handle your events, implement the nested interface in your Activity or define it as an anonymous mehtod and pass it your implementation of the oOnClickListener

Remember that hardware key events are always delivered to the View currently in focus. They are dispatched starting from the top of the View hierarchy, and then down, until they reach the appropriate destination.  If your View currently has focus, then  you can see the event travel throughthe dispatchKeyEvent() methods. As an alternative to capturing keyevents through your View, you can also receive all the events inside your Activity with onKeyDown() and onKeyUp()

Also when thinking about text input for your application, remember that many devices only have software input methods such methods are not required to be key-based; some may use voice input handwrigting and so on. Even if an input methods presents a keyboard-like interface, it will generally not trigger the onKeyDown() family of events. You should never build a UI that requires specific key presses to be controlled unless you want to limit your application to devices with a hardware keyboard. In particular, do not relly on these methods to validate input when the user presses the return key; instead use action like IME_ACTION_DONE to signal the input methods how your applciation expects to react, so it may change its UI in a meangful way. Avoid assumptions about how a software in put methods should work and just trust it to supply already formateed text to your application. 

Android will call event handlers first an then the appropriate default handlers from the class definition second. As such, returning true from these event listeners will stop the propagation of the event to other event listeners and will also blck the callback to the default event handler in the View. So be certain that you want to terminate the event when you return true.

## EventHandlers. 
