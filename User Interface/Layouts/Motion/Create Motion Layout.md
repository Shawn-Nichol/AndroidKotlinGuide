# How to create a MotionLayout

## 1) Dependencies.
```
dependencies {
    implementation 'androidx.constraintlayout:constraintlayout:2.0.0-beta1'
}
```

## 2) Create a MotionLayout
In the XML layout create a `MotionLayout`, you can convert a `ConstraintLayout` to `MotionLayout`. 


## 3) Create a MotionScene
A `MotionScene is an XML resource file that contains all of the motion descriptions for the corresponding layout. To keep layout information separate from motion descriptions, each `MotionLayout` references a separate MotionScene. note that defintions in the MotionScene takes precendence over any similar definitions in the `MotionLayout`. 

`app:LayoutDescription` refereences a `MotionScene`.
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
