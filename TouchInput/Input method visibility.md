# Handling input method visibility
When input focus moves into or out of an editable text field, Android shows or hides the input method (such as the on-screen keyboard) as appropriate. The system also makes decisions about how your UI and th text field appear above the input method. For example, when the vertical space on the screen in constrained, the text field might filll all space above the input method. For most apps, these defalt behaviors are all that's needed. 

In some cases, though you might want to more directly control the visibility of the input method and specify how you'd like your layout to appear when the inputmethod is visible. This lesson explains how to control and respond to the input method visibility. 

## Show the input method when the activity starts
Although Android gives focus to the first text field in your layout when the ativity starts, it does not show the input method. This behavior is appropriate becuase entering text might not be the primary task in the activity. However, if entering text i indeed the primary task (such as in a loing screen), then you probably want the input method to appear by default. 

To show input method when your activity starts, add the `android:windowSoftInputMode` attribute to the <ativity> elemetn with the "stateVisible" value 
```
<application ... >
    <activity
        android:windowSoftInputMode="stateVisible" ... >
        ...
    </activity>
    ...
</application>
```

## Show the input method on demand
If there is a methd in your activity's lifecycle wher eyou want to ensur that the input method is visible, you can use the `InputMehtodManger` to show it. 

fun showSoftKeyboard(view: View) {
    if (view.requestFocus()) {
        val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
        imm.showSoftInput(view, InputMethodManager.SHOW_IMPLICIT)
    }
}

## Specify how the UI should respond
When the input method appears on teh screen, it reduces the amount of space available for your app's UI. THe system makes a decision as to how it should adjust the visible portion of your UI, but it might not get it right. TO ensur the best behavior for your app, you should specify how you'd like the system to display your UI in the remaining space. 

To declare you preferred treatment in an activity, use the `andorid:windowSoftInputMode` attribute in your manifest's `<activity>` element with one of the adjust values. 
```
<application ... >
    <activity
        android:windowSoftInputMode="adjustResize" ... >
        ...
    </activity>
    ...
</application>
```
