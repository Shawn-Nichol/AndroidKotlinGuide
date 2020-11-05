# Delayed loading of Views
Sometimes your layout might require complex views that are rarely used. Whether they are itme detials, progress indicators, or undo messages, you cn reduce memory usage and speed up rendering by loading the views only when they are needed. 

Deferring laoding resources is an improtantn techinque to use when you have complex views that your app might need in the future. You can impelement this technique by definding a ViewStub for those complex and rarely used views. 

## Define a ViewStub
`ViewStub` is alightweight view with no dimension that doesn't draw antything or participate in the layout. As such it's cheap to inflate and cheap to leave in a view hierarchy. Each `ViewStub` simply needs to include the android:layout attribute to specify the layout to inflate. 

The following `ViewStub` is  for a traslucent prgoress bar overlay. It should be visible only when new items are being inproted in to the app. 

```
<ViewStub
    android:id="@+id/stub_import"
    android:inflatedId="@+id/panel_import"
    android:layout="@layout/progress_overlay"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:layout_gravity="bottom" />
```

## Loading the ViewStube Layout
When you want to laod the layout specified by the ViewStube, either set it visible by calling setVisibility(View.VIsible or call inflate(). 
```
findViewById<View>(R.id.stub_import).visibility = View.VISIBLE
// or
val importPanel: View = findViewById<ViewStub>(R.id.stub_import).inflate()
```
Once visible/inflated, the `ViewStub` element is no longer part of the view hierachy. It is replaced by the inflated layout and the ID for the root view of that layout is the one specified by the `android:inflatedId` attribute of the ViewStub. THe ID `android:id` specified for the `ViewStub` is valid only until the `ViewStub` layotu is visible/inflated.
