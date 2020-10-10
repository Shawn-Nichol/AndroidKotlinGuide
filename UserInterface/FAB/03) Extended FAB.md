## 1) Add Dependency
```
implementation 'com.google.android.material:material:1.2.1'
```

## 2) Coordinator Layout
Add a coordinator layout to the layout file, this will be the parent of the FAB. The FAB has to be a first generation Child of the CoordinatorLayout or some of the action don't work as intented
```
<androidx.coordinatorlayout.widget.CoordinatorLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
```

3) ADD Extended FAB to layout
layout_extneded doens't appear to work here. 
```
        <com.google.android.material.floatingactionbutton.ExtendedFloatingActionButton
            android:id="@+id/my_fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|right|end"
            android:layout_margin="16dp"
            android:onClick="@{()-> binding.addUser()}"
            android:text="MyFAB"

            app:icon="@drawable/ic_plus"
            app:backgroundTint="@color/design_default_color_error"
            app:rippleColor="#FF8C00"
            app:strokeWidth="10dp"
            app:strokeColor="@color/colorAccent"

            />


```

