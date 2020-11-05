# KeyTimeCycle
KeyTimeCycle runs a specified amount of cycles per second. 


MoitionScene
```
<?xml version="1.0" encoding="utf-8"?>
<MotionScene xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:motion="http://schemas.android.com/apk/res-auto">


    <Transition
        android:id="@+id/transition_up_down"
        motion:autoTransition="animateToEnd"
        motion:constraintSetStart="@id/const_top"
        motion:constraintSetEnd="@id/const_bottom"
        motion:duration="5000"
        motion:motionInterpolator="linear">

        <OnClick
            motion:clickAction="toggle"
            motion:targetId="@+id/const_btn_three"/>
          
        // With out setting a start and stop KeyTimeCycle the the animation will continue on in the last position.   
        <KeyFrameSet>

            <KeyTimeCycle
                motion:motionTarget="@id/const_btn_three"
                motion:wavePeriod="0"
                motion:framePosition="0"
                android:translationY="0dp"
                motion:waveOffset="0dp"
                motion:waveShape="sin"/>

            <KeyTimeCycle
                motion:motionTarget="@id/const_btn_three"
                // THe number of cycles per second, a
                motion:wavePeriod="1"
                motion:framePosition="10"
                android:translationY="50dp"
                motion:waveOffset="0dp"
                motion:waveShape="sin"/>

            <KeyTimeCycle
                motion:motionTarget="@id/const_btn_three"
                motion:wavePeriod="0"
                motion:framePosition="100"
                android:translationY="0dp"
                motion:waveOffset="0dp"
                motion:waveShape="sin"/>
        </KeyFrameSet>
    </Transition>

    <ConstraintSet android:id="@+id/const_top">
        <Constraint
            android:id="@+id/const_btn_three"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="200dp"
            motion:layout_constraintTop_toTopOf="parent"
            motion:layout_constraintStart_toStartOf="parent">
            >

            <CustomAttribute
                motion:attributeName="BackgroundColor"
                motion:customColorValue="#9999FF" />
            <CustomAttribute
                motion:attributeName="Text"
                motion:customStringValue="Down"/>
        </Constraint>
    </ConstraintSet>

    <ConstraintSet android:id="@+id/const_bottom">
        <Constraint
            android:id="@id/const_btn_three"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="200dp"
            motion:layout_constraintTop_toTopOf="parent"
            motion:layout_constraintEnd_toEndOf="parent">
            <CustomAttribute
                motion:attributeName="BackgroundColor"
                motion:customColorValue="#D8aB60"/>
            <CustomAttribute
                motion:attributeName="Text"
                motion:customStringValue="Up"/>
        </Constraint>
    </ConstraintSet>
</MotionScene>
```
