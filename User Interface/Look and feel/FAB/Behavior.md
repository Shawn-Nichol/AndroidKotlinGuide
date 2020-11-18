
# FAB Behavior
This class is called from the FAB attributes in XML layout. 
```
        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/my_fab"
            ...
            app:layout_behavior=".playerlist.ui.FABBehavior"/>

```

## Create FAB Behavior class
Since the class is called from XML it will need a context and attribute arrgument. 
```
class FABBehavior(context: Context, attrs: AttributeSet) : FloatingActionButton.Behavior() {
```

## onScrolling fun
Override `onStartNestedScrolling`, this fun is called when the user starts scrolling.
```
    override fun onStartNestedScroll(
        coordinatorLayout: CoordinatorLayout,
        child: FloatingActionButton,
        directTargetChild: View,
        target: View,
        axes: Int,
        type: Int
    ): Boolean {
        return if (axes == ViewCompat.SCROLL_AXIS_VERTICAL) {
            true
        } else {
            super.onStartNestedScroll(
                coordinatorLayout,
                child,
                directTargetChild,
                target,
                axes,
                type
            )
        }
    }
```


## Nested scroll
Override `onNestedScroll`, this fun is called when the user is scrolling. 
```
    override fun onNestedScroll(
        coordinatorLayout: CoordinatorLayout,
        child: FloatingActionButton,
        target: View,
        dxConsumed: Int,
        dyConsumed: Int,
        dxUnconsumed: Int,
        dyUnconsumed: Int,
        type: Int,
        consumed: IntArray
    ) {
        super.onNestedScroll(
            coordinatorLayout,
            child,
            target,
            dxConsumed,
            dyConsumed,
            dxUnconsumed,
            dyUnconsumed,
            type,
            consumed
        )

        // This statement will hide the Fab on a downward swipe and make the fab visible on an upward
        // swipe.
        if (dyConsumed > 0 && child.visibility == View.VISIBLE) {
            child.hide(object : FloatingActionButton.OnVisibilityChangedListener() {
                override fun onHidden(fab: FloatingActionButton) {
                    super.onHidden(fab)
                    fab.visibility = View.INVISIBLE
                }
            })
        } else if (dyConsumed < 0 && child.visibility != View.VISIBLE) {
            child.show()
        }
    }
```

## onStopScrolling
Override `onStopNestedScrolling`, the fun is called after the user has stopped scrolling. 
```
    override fun onStopNestedScroll(
        coordinatorLayout: CoordinatorLayout,
        child: FloatingActionButton,
        target: View,
        type: Int
    ) {
        super.onStopNestedScroll(coordinatorLayout, child, target, type)
        ...
    }

```
