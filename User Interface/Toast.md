# Toast
A toast provides simple feedback about an operationin a small popup. It only fills the amount of space required for the message and the current activity remains visible and interactive. Toast automatically disappear aftera timerout.

Toasts are not clickable. If users response to a status message si requiredd, consider instead uings a Notfication.

## The basics
First,instantiate a Toast object with one of the `makeText()`. This method takes thress parameters the appliation context, the text message, and the duration of the toast. It retursn a properly initialized Toast object. you can display the toast notification with `show()`
```
Toast.makeText(context, "MY text", Toast.LENGTH_SHORT).show()
```

## positioning your toast
A standard toast notification appears near the bottom of the screen centered horizontally. You can change this poistion witht he setGravitity(int, int, int). This accepts three paramaeters a gravity constant, and x-position offset, and a y-position offset.

```
toast.setGravity(Gravity.TOP or Gravity.LEFT, 0, 0)

```

If you want to nudge the position ot the right, increase the value of the second parameter. to nudge it down, increase the value of the lst parameter.

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

Notice that the ID of the LinearLayout element is "custom_toast_container". You must use this ID and the ID of the XML layout file "custom_toast" to inflate the layout. 

```
val inflater = layoutInflater
val container: ViewGroup = inflater.inflate(R.layout.custom_toast, container)
val layout: ViewGroup = inflater.inflate(R.layout.custom_toast, container)
val text TextView = layoutfindViewByid(R.id.text)
text.text = "This a custom toast."
with(Toast(applicationContext)) {
  setGravity(Gravity.CENTER_VERTICAL, 0, 0)
  duration = Toast.LENGTH_LONG
  view = layout
  show()
}
```

First, retrieve the layoutinflater with `getLayoutInflater()` and then inflate the layout from XML using `inflate(int, ViewGroup)`. The first parametere is the layout resource ID and the seconed parameter is the root view. YOu can use this inflated layout to find more View objects in the layout, so now capture and define the content for theimageView and TextView elemeents. Finally, create a new Toast with `Toast(Context` and set some properties of the toast, such as the gravity and duration. Then call setView(View0 and pass it the infalted layout. You can now display the toast with your custom layout by calling `show()`.
