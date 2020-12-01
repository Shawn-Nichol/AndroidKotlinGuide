# Toast
A toast provides simple feedback about an operationin a small popup. It only fills the amount of space required for the message and the current activity remains visible and interactive. Toast automatically disappear aftera timerout. Toasts are not clickable. If users response to a status message si requiredd, consider instead uings a Notfication.

## The basics
- First, instantiate a Toast object with one of the `makeText()`. 
- This method takes thress parameters the appliation context, the text message, and the duration of the toast. It returns a properly initialized Toast object.
- Display the toast notification with `show()`.
```
Toast.makeText(context, "MY text", Toast.LENGTH_SHORT).show()
```

## positioning your toast
Toast notifications appear near the bottom of the screen centered horizontally. You can change this poistion with the `setGravitity(int, int, int)`. This accepts three paramaeters a gravity constant, and x-position offset, and a y-position offset.

```
toast.setGravity(Gravity.TOP or Gravity.LEFT, 0, 0)

```

## Creating a Custom Toast
If a simple text message isn't enough, you can create a customized layout for your toast notification. To create a custom layout, define a View layout, in XML or in your application code, and pass the root View object to the setView method.

The following snippet contains a customized layout for a atoast notfication
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/custom_toast_container"
              android:orientation="horizontal"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent"
              android:padding="8dp"
              android:background="#DAAA"
              >
    <ImageView android:src="@drawable/droid"
               android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:layout_marginRight="8dp"
               />
    <TextView android:id="@+id/text"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:textColor="#FFF"
              />
</LinearLayout>
```

## Custom Layout
- First, retrieve the layoutinflater with `getLayoutInflater()` 
- inflate the layout from XML using `inflate(int, ViewGroup)`. The first parametere is the layout resource ID and the seconed parameter is the root view.
- Use inflated layout to find more View objects in the layout, 
- Define the content for the elements.
- Create a new Toast with `Toast(Context)` and set the properties of the toast. 
- Call setView(View) and pass it the infalted layout. 
- Display the toast with your custom layout by calling `show()`.


```
val inflater = layoutInflater
val layout: View = inflater.inflate(
    R.layout.custom_toast,
    requireActivity().findViewById(R.id.custom_toast_container) as ViewGroup?
)

 Toast(context).apply {
    duration = Toast.LENGTH_LONG
    setView(layout)
    setGravity(Gravity.CENTER_VERTICAL, 0, 0)
    show()
}

```


