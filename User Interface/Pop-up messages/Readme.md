# Pop-up messages 
There are many situations wher you migh want your app to show a quick message to the user iwthout necessarily waiting fo the user to respond. Fore example, when a user performs an action like sending an email or deleting a file, your app should show a quick conifrmation to hte user. Often the users doesn't need to repsond to the message. The message needs to be prominent enough that the user can see it, but not so prominent that it prevents the user from working with the app.  

Android provides the `Snackbar` widget for this common use case. A Snackbar provides a quick pop-up message to the user. the current activity remains visible and interactive while the snackbar is disaplayed. After a short time, the snackbar automatically dismisses itsself. 

This class teaches you how to use Snackbar to show po-up messages. 


## Build and display a pop-yp message
you can use a `Snackbar` to display a brief message to the user. the message automtaically goes away after a short period. A snackbar is ideal for brief messages that the user doesn't necessarily need to act on . For example, an email app ccould use a Snackar to tell the user that the app successfully sent an eamil. 

## Use a corrdinatorLayout
A snackbar is attached to a view. The Snackbar provides basic funcionality if it is attached to aany object derived from the view class, such as any o f the common layout objects. However, if the sNackbar is attached to a CoordiantorLayout, the Snackbar gains additional features. 

- The user can dismiss the snackbar by swiping it away. 
- The layotu moves some other UI elemetns whent eh snackbar appears. For example, if the layout has a FloatingActionButton, the layout moves the button up when it shows a Snackbar, instead of drawing the snackbar on top of the button. 

Teh `CoordinatorLayout` class provides a susperset of the functionality of FrameLayout. If your app already uses a `FrameLayotu`, you can just replace that layout with a `CoordinatorLayout` to enable the full `Snackbar` functionality. If your app uses other layout objects, the simplest thing todo is wrap your existing layout element in a CoordinatorLayout 

```
<android.support.design.widget.CoordinatorLayout
    android:id="@+id/myCoordinatorLayout"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Here are the existing layout elements, now wrapped in
         a CoordinatorLayout -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!-- …Toolbar, other layouts, other elements… -->

    </LinearLayout>

</android.support.design.widget.CoordinatorLayout>
```
Make sure to set an android id tag ofr you rCoordinatorLayout. You need the layout's ID when you display the message. 


## Display message
There are two steps to displaying a message. First you creata a snackbar object with the message  text. Then you call that object's show() method to display the message to the user. 

### Creating a Snackbar object
Createa a `Snackbar` object by calling the static `Snackbar.make()`. When you creat the `Snackbar`, yo us pecify both the message it displays, and the length of time to show the message

```
val mySnackbar = Snackbar.make(view, StringId, duration)
```

view: 
The View to attcach the snackbar to. The method actually searches up the view hierarchy from the passed view until it reaches either a coordinatorlayout, or thw window decor's content view. Ordinarily, it's simplest to jsut pass the `CoordinatorLayout enclosing youg content. 

stringId
The resource ID of the message you watn to dispaly. This can be formattted or unformatted text. 

duration: 
The length of time to show the message. This can be either LENGTH_SHORT or LENGTH_LONG.

## Showing the message to the user. 
Once you have created the snackbar, call its show method to display the snackbar to the user: 

```
mysnackbar.show()
```

The system does not show multiple Snackbar object at the same time, so if the view is currently displaying another Snackbar, the ssytem queues your `Snackbar` and displays it after the curretn `Snackbar` expires or is dismissed. 

If you just want to shwo a message to the user and won't need to call any o the `Snackbar` objects utility methods, you dont' need to keep the reference to the `Snackbar` after you call show() .For this reason, it's common to sue method chaing to create and show a snackbar in one statement. 

```
Snackbar.make(findViewById(R.id.mycoordinatorLayout), R.string.email_sent, Snackbar.LENGTH_SHORT).show()
```

Add an action to a message 
You can adda n action to a `Snackbar`, allowing the user to respond to  your message. If you add an action to a `Snackbar` puts a button next to the message text. Th euser can trigger your action by pressing the button for example an email app might put an undo button o nits email archived message if the user clicks the undo button, the app takes the email back out of the archvie. 

To add an action to a `Snackbar` message, you need to define a listener object that implements the `View.OnClickListner` interface. The system calls your listener's `onClick()` method if the user clicks on the message action. 

```
class MyUndoListener : View.OnClickListener {
  fun onClick(v: View) {
    // Code to undo the user's last action
  }
}
```

Use one the `SetAction()` methods to attach the listener to your `Snackbar`. Be sure to attach the listener before you call `show()`, as shown in this code sample. 
```
val mySnackbar = Snackbar.make(findViewById(R.id.myCoordinatorLayout),
                               R.string.email_archived, Snackbar.LENGTH_SHORT)
mySnackbar.setAction(R.string.undo_string, MyUndoListener())
mySnackbar.show()
```
