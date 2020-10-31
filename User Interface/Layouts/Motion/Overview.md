# Motion and widget animation with MotionLayout
`MotionLayout` is a layout type that helps you manage motion and widget animation in your app. 

MotionLayout is a subclass of `Constraaintlayout` and builds upon its rich layout capabilities. As par of the Constraintlayout library, `MotionLayout` is available as a support library and is backwards-compatible.

MotionLayout bridges the gap between layout transitions and complex motion handling, offering a mix of features between the `property animation framework`, `TransitionManager` and `CoordinatorLayout`. 

In addition to descibing transitions between layouts, MotionLayotu lets you animate any layout properites, as well. Moreover, it inherently supports seekable transitions. THis means that you can instantly show any point within the transition based on some conditions, such as touch input `MotionLayout` aslo supports keyframes, nebaling fully customized transitions to suit your needs. 

`MotionLayout` is fully declarative, maning that you can descibe any transition in XML, no matter how complex.

Note: `MotionLayout` works only with its direct children. it does not support nested layout hierarchies or activity transitions. 

## Design considerations
`MotionLayout` is intended to move, resize and animate UI elements with which users interact, such as buttons and title bars. Motion in your app should not be simply a gratuitous special effect in your application. It should be sued to  help users understand what your application is doing. For more information on desgning your app  with motion see the Materail Desing sectiono on Understanding motion. 

## Getting Started
1. Dependencies
```
dependencies {
    implementation 'androidx.constraintlayout:constraintlayout:2.0.0-beta1'
}
```

2. Create a `MotionLayout` file `MotionLayout` is a sbuclass of ConstraintLayout, so you can transform any existing `ConstraintLayout` into a `MotionLayout` by replacing the class name in your alyout resource file. 

```
<!-- before: ConstraintLayout -->
<androidx.constraintlayout.widget.ConstraintLayout .../>
<!-- after: MotionLayout -->
<androidx.constraintlayout.motion.widget.MotionLayout .../>
```

```
<?xml version="1.0" encoding="utf-8"?>
<!-- activity_main.xml -->
<androidx.constraintlayout.motion.widget.MotionLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/motionLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layoutDescription="@xml/scene_01"
    tools:showPaths="true">

    <View
        android:id="@+id/button"
        android:layout_width="64dp"
        android:layout_height="64dp"
        android:background="@color/colorAccent"
        android:text="Button" />

</androidx.constraintlayout.motion.widget.MotionLayout>
```

3. Create a MotionScene: In the previous `MotionLayout` example, the `app:layoutDescription` attribute references a MotionScene. A MotionScene is an XML resource file that contains all of the motion descriptions ofr the corresponding layout. To keep layout information separate from motion descriptions, each MotionLayout references a separate MotionScene. Notet that definations in tneh MotinoScene take precendence over any similar definitions in the Motionlayout. 

Here's an exmaple MotionScene file that descibes th basic horizontal motion.

```
<?xml version="1.0" encoding="utf-8"?>
<MotionScene xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:motion="http://schemas.android.com/apk/res-auto">

    <Transition
        motion:constraintSetStart="@+id/start"
        motion:constraintSetEnd="@+id/end"
        motion:duration="1000">
        <OnSwipe
            motion:touchAnchorId="@+id/button"
            motion:touchAnchorSide="right"
            motion:dragDirection="dragRight" />
    </Transition>

    <ConstraintSet android:id="@+id/start">
        <Constraint
            android:id="@+id/button"
            android:layout_width="64dp"
            android:layout_height="64dp"
            android:layout_marginStart="8dp"
            motion:layout_constraintBottom_toBottomOf="parent"
            motion:layout_constraintStart_toStartOf="parent"
            motion:layout_constraintTop_toTopOf="parent" />
    </ConstraintSet>

    <ConstraintSet android:id="@+id/end">
        <Constraint
            android:id="@+id/button"
            android:layout_width="64dp"
            android:layout_height="64dp"
            android:layout_marginEnd="8dp"
            motion:layout_constraintBottom_toBottomOf="parent"
            motion:layout_constraintEnd_toEndOf="parent"
            motion:layout_constraintTop_toTopOf="parent" />
    </ConstraintSet>

</MotionScene>
```

Note the following

- `<Transition>` contains the base definition of the motion.
   -`motion:constraintSetStart and motion:constraintSetEnd are references to the endpoints of the motion. These endpoints are defined in the `<ConstraintSet>` elements later in the MotionScene
   
   - `motion:duration` specifiese the number of milliseconds that it takes for th emotion to complete. 
- `<OnSwipe>` lets you control the motion via touch
  - `motion:touchAnchorId` referes to the view that you can swipe and drag. 
  - 'motion:touchAnchorSide` means that we are dragging the view from the right side. 
  - 'motion:dragDirection refers to the progress direction fo the drag. For example, `motion:dragDirection="dragRight"` means that progress increases as you drag to the right. 
- `<ConstraintSet> is wher you define th evarious constraints that descibe your motion. In this exmaple, we define one `ConstraintSet for each endpoint of our motion. TThese endpoints are centered vertically (via app:layout_constraintTop_toTopOf="parent" and app:layout_constraintBottom_toBottomOf="parent"). Horizontally, the endpoints are at the far left and right sides of the screen.


## Interrpolated attributes
Within a MotionScene file, `ConstraintSet` elements can contain additional attributes that are interpolated during transition. In addtion to postionand bounds, the following attributes are interpolated by `MotionLayout`
- alpha
- visibility
- elevation
- rotation, rotationX, rotationY
- translationX, translaytionY, translationZ
- scaleX, scaleY

## Custom attributes
Withina `<Constraint>L`, you can use the `<CustomAttribute>` element to specify a transition for attributes that aren't simply related to position or view attributes. 

```
<Constraint
    android:id="@+id/button" ...>
    <CustomAttribute
        motion:attributeName="backgroundColor"
        motion:customColorValue="#D81B60"/>
</Constraint>
```


- A `<CustomAttribute>` contains tow attributes of its own. 
  - `motion:attributeName` is required and must match an object with getter and setter methods. The getter and setter must match a specific pattern. For exmaple, `backgroundColor` is supported, since our view has underlying `getBackgroundColor() and `setBackgroundColor()
  - The other attribute you must provide is based on teh value type. Choose from the following
  -  motion:customColorValue for colors
  - motion:customIntegerValue for integers
  - motion:customFloatValue for floats
  - motion:customStringValue for strings
  - motion:customDimension for dimensions
  - motion:customBoolean for booleans

## Addition MotionLayot attributes
In addition to attributes in the xampel above, `MotionLayout` has other attributes.
- app:applyMotionScene="boolean" indicates whether to apply the MotionScene. The default value for this attribute is true. app:showPaths="boolean" indicates whether to show the motion paths as the motion is running. The default value for this attribute is false.
- app:progress="float" lets you explicitly specify transition progress. You can use any floating-point value from 0 (the start of the transition) to 1 (the end of the transition).
- app:currentState="reference" lets you specify a specific ConstraintSet.
app:motionDebug lets you display additional debug information about the motion. Possible values are "SHOW_PROGRESS", "SHOW_PATH", or "SHOW_ALL".

## a
