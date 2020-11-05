# Multiple View animation. 
The `MotionLayout` can only handle one Transition sequence, if you wish to have separate animations for multiple views you need to create `MotionLayout` for each animation. 

```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
      ...
    </data>

    <!-- [databinding] {"msg":"Only one layout element with 1 view child is allowed.  -->
    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <androidx.constraintlayout.motion.widget.MotionLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layoutDescription="@xml/motion_scene_01"
            tools:context=".menu.contextual.FragmentContextualOne"
            tools:showPath="true">

            <Button
                android:id="@+id/btn_one"
                android:layout_width="64dp"
                android:layout_height="64dp"
                ... />


        </androidx.constraintlayout.motion.widget.MotionLayout>

        <androidx.constraintlayout.motion.widget.MotionLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layoutDescription="@xml/motion_scene_02">

            <Button
                android:id="@+id/btn_two"
                android:layout_width="64dp"
                android:layout_height="64dp"
                ... />
        </androidx.constraintlayout.motion.widget.MotionLayout>
        
        <androidx.constraintlayout.motion.widget.MotionLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layoutDescription="@xml/motion_scene_03">

            <Button
                android:id="@+id/btn_three"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                ... />
        </androidx.constraintlayout.motion.widget.MotionLayout>
    </FrameLayout>

</layout>
```
