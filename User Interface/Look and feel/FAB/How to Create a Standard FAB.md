

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

3) ADD FAB to layout
```
<com.google.android.material.floatingactionbutton.FloatingActionButton
    android:id="@+id/my_fab"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_margin="16dp"

    // Position the FAB on the screen
    app:layout_anchorGravity="bottom|right|end"
    
    // Anchor it to a position
    app:layout_anchor="@id/container"
    
    // onClick action (viewBinding)
    android:onClick="@{() -> binding.addUser()}"
    
    // Drawable image displayed in the FAB
    android:src="@drawable/ic_plus"
    
    // Linked the FAB behavior class, see how to setup behavior class. 
    app:layout_behavior="com.example.practicerecyclerview2.MyFABBehaviorclass"
   
    // Color of FAB
    android:backgroundTint="@color/design_default_color_error"
    
    // FAB border
    app:backgroundTint="@color/colorAccent"
    app:borderWidth="10dp" 
    
    // Color FAB when press
    app:rippleColor="#FF8C00"
    
    />

```




