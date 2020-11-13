## Create Shadows and clip views. 
Material deisgn introduces elevation for UI elements. Elevation helps users understand the relative importance of each elemetn and focus their attention to the task at hand. 

The elevation of a view, represented by the Z property, determines the visual appearance of its shadow: views with higher Z values cast larger, softer shadows,> Views with higher Z values occulde views with lower Z values: however, th eZ value of a view does not affect the view's size. 

Shadows are drawn by the parent of the elevated view, and thus subect to standard view clipping, clipped by the parent by default. 

Elevation is also useful to create animations where widgets temporarily rise aobove the view plane when pefroming some action. 

## Assign Elevation to your views
The Zvalues fo a view has two compenents
- Elevation: the static compeont
- Translation: The dynamic component used for animations 
z = elevation + translationZ


To set the default resting elevation of a view, use the `android:elevation` attribute in the XML layout. To set the elevation of a view in teh code of an activity, use the `View.setelevation()`.

The new `ViewPropertyAnimator.z()` and `VewPropertyAnimator.translationZ()` methods enabled you to easily animate the elevation of a views. For more information see th API reference for `ViewProeprtyAnimator` and the Property Animation developer guide. 

You can also use a `StateListAnimator` to speify these animations in a declarative way. Thisis especially useful for cases where state changes trigger animations, like when a user presses a button. For more information see Animate View state chagnes. 

The Z values are measured in dp

## Customize view Shadows and Outlines
The bounds of a view's background drawable determine the default shape of its shadow. Outlines represent th eouter shape of a graphics object and define the  ripple area for touch feedback. 

Consider theis view, defined with a background drawable.
```
<TextView
    android:id="@+id/myview"
    ...
    android:elevation="2dp"
    android:background="@drawable/myrect" />
```

The backgroun drable is defined as a rectangle wiht rounded corners;
```
<!-- res/drawable/myrect.xml -->
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle">
    <solid android:color="#42000000" />
    <corners android:radius="5dp" />
</shape>
```

The view casts a shadow with rounded coreners, since the background drawable defines the view's outline. Providing a custom outline overrides thedefalut shape of a view's shadow. 

To define a custom outline for view in your coe:
1. Extend the `ViewOutlineProvider class. 
2. Override the `getOutline()` method. 
3. Assign the new outline provider to your view with the `View.setOutlineProvider() method. 

You can create oval and rectangular outlines with rounded corners using the mehtods in the Outline class. The default outlin eprovider for views obtains the outline from the view's background. To prevent a view from casting a shadow, set its outline provider to null. 

## Clip Views
Clipping views enables you to easily change the shape of a view. You can clip views for consistency with other design elements or to change the shape of a view in response to user input. You can clip a cview to its outline are using the `View.setClipToOutline(0` mehtod or the `android:clipTooutline` attribute. ONly rectangle, circle, and round rectangle outllines support clipping, as determined by the `Outline.canClip()`

To clip a view to the shape of a drawable, set the drawable as the background of the view and call the `View.setClipTooutline(0`

Clipping views is an expensive operation, so don't animat the shape you use to clip a view. To achieve this efeect use the Reveal Effect animation. 
