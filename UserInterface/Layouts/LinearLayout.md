Linear layout is a viewgroup that aligns all children in a single direction, vertically or horizontally.

You can specify the direction with the android:orientation attribute. 

## Layout Weight
Linearlayout also supports assigning a weight to individual children with the android:layout_weight attribute. This attribute assigns an importance value to a view in terms of how much space it should occupy on the screen. A larger weight value allows it to expand to fill any remaining space in the parent view. Child views can specify a weight value and then any remaining space in the parent veiw. Child views can specify a wieght value, and then any remaining space int he veiw group is assigned to children in the proprtion of their declared wieght.  Default weight is zero. 

Equal distribution, set the all child view's weight to same number and the screen will be divided evenly amongst all the views. 

Unequal distribution
You can also create a linear layout where the child elements use different amounts of space on the screen. 
