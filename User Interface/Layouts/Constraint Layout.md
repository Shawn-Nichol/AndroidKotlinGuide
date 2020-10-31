# What is Constaintlayout
`ConstaintLayout` allows you to create a large and complex layouts with a flat view hierarchy (no nested view groups). It's similar to `RelativeLayout` in that all views are laid out according ot relatioinships betweeen sibling views and th parent layout, but it's more flexible than `RelativeLayout` and easier to use with the Layout Editor. 

All the power of `ConstraintLayout` is available directly from the Layout Editor's visual tools, becuase the layout ApI and the Layout Editor were specially built for each other. So you can build  your layout with ConstraintLayout entierly by drag-and-dropping instead of editing the XML.

## Overrivew
To define a view's position in `ConstraintLayout`, you must add at least one horizontal and one vertical constraint for the view. Each constraint represents a connection or alignemtn to another view, the parent layout, or an invisible guidleline. Each constraint defines the view's position along either the vertical or horizontal axis; so each view must have a mimium of one constraint for each axis, but oten more are necessary. 

When you dorp a view into the LayoutEdiotr, it stays where you leave it even if it has no constraints. However, this is only to make editing easier; ifa view has no constrains when you run your layout on a device, it is drawn at position[0,0]

## add ConstraingLayout 
1. Ensure you have the maven.google.com repository decoared in module-level build.gradle file
```
repositories {
    google()
}
```

2. Add the library as a dependency in the same build.gradle file.
```
dependencies {
    implementation "androidx.constraintlayout:constraintlayout:2.0.3"
}
```

## Add or remove a constraint
1. Drag a view in a `ConstraintLayout`, it displays a bounding box with square resizing handles on each corner and circular constraint handles on each side. 
2. Click the view to select it
3. Drag a constraint handle to anchor point
   Click on the Create a connection button in the layout section of the Attributes. 
   
When the constraint is created, the editor gives it a `default margin` to separate the two views. When creating constraints, rember the follwoing rules
- Every view must have at least two constraints: one horizontal and one vertical. 
- you can create constraints only between a constraint handle and an anchor point that share the same plane. So a vertical plane of a view cna be constained onlyl to another vertical plane; and baselines can constain onlly  to other baselines. 
- Each onstraint handle can be used for just one constraint, but you can create multiple constraints to the sam anchor point. 

You can delete a constraint by doin g any of the following:
- Click on a constraint to select it, and then press `Delete`
- Press and hold `Control`, and then click ona constraint anchor. Note that the constraint turns red to indicate hat you can click to delete it. 

## Alignment
Align the dege of a view to the same edge of another view. you can offset the alignemtn by dragging hte view inward form the constratint. You can also select all the views you want to align and then click `Align` in the toolbar to select the alignment type. 

## Base alignemtn
Align the text baseline of a view to the text baseline of another view.  To createa a baseline constraint, right-click the text view you want to constrain and then click `Show Baseline`. Then click on the text baseline  and drag the line to another baseline.

## Constraint to a guideline
you can add a vertical or horizontal guideline to which you can constain views, and the guidleline will be invisible to app users. you can position the guidleline within the layout based on either dp units or precent, relative to the layout's edge. 

To create a guideline, click Guidelines in the toolbar, and then click either add vertical Guideline or add horizontal Guideline. Drag the dotted line to reposition it and click the circle at the edge of the guideline to toggle the measuremtn mode. 

## Constrain to a barrier
Simlar to a guidline, a barrier is an invisible line that ou can constrain views to . Except a barrier does not define its own position; instead the barrier position moves based on teh position of views contained witin it. This is useful when you want to constrain a view to a set of views rather than to one specific view. 

To Create barrie
1. CLick `Guidelines`  in the toolbar and then click add vertical barrier or add Horizontal barrier. 
2. In teh componet tree window select the views you want inside th e barrier and drag them into the barrier component. 
3. Select the barrier from teh Component tree, open the attributes window, and then set the barrierDirection

now you can createa constraint form anothe view to the barrier. you can also constain views that are inside the barrier to the barrier. This way, you can ensure that all views in the barrier always align to each other, even if you don't know which view will be the longest or tallest. 

You can also include a guideline insde a barrier to ensure a mimium posittion for the barrier. 

## Adjust the constraint bais. 
When you add a constaint to both sides of a view, the view becomes centered betweeen the two constraints witha bias of 50% by default. you can adjust he bias by draggin the bias slider in the attributes window. 

## Adjust the view size
You can use the corner handles to resize a view, but this hard codes the size so the view will not resize for different content or screen sizes. To select a different sizing mode, click a view and open the Attributes window on the right side of the editor. 

Near the top of the attributes window si the view inspector, which includes controls for several layout attributes. You can change the way the height and width are calculated by clicking the symbols indicated. 
- Fixed: you specify a apecific dimension in the text box below or by resizing the view in the editor. 
- Wrap Content: The view expands only as much as needed to fit its contents. 
- Match Constraints: The view expands as much as possible to meet the constraints on each side. However, you cna modify that behavior with the following attributes an values. 
  - layout_constraintWidth_default
  - Spread: Epands the view as much as possible to meet the constraints on each side. This is the default behaivor. 
  - Wrap Expands the view only as much as needed ot fit its contents, but still allows the vieww to be smaller than that if the constraints require it. So the difference between this and using WrapContent, is that setting the width to wrap conent forces the width to alwasy exactly match the content width; whereas using Match Constraints with layout_constraintWidth_Default set to wrap also allows the view to be smaller than the content wdith. 
  
  ## Set size as a ratio
  you can set the view size to a ratio such as 16:9 if at least one of the view dimensions is set to match constraints 0dp. To enable the ratio, click Toggle Aspect Ratio constrant adn then enter the width:height ratio in the input that appears. 
  
  If both the width and height are set to match constraints, you can click Toggle Aspect Ratio Constraint to select wich dimension is ased on a ratio ofthe tother. The view inspector indicaates which is set as a raito by connecting the corresponding edges with a solid line. 
  
## adjust View margins. To ensure that all your view are evenly spaced.click Margin 16dp in the toolbar to select the dfault margin for each view that you add to the layout. Any change you amek to the default amrgin applies only to the views you add form then on

You can control the margin for each view in attributes window by clicing the nubmer on the line that represents each constraint

## Control Linear groups with a chain. 
A chian is a group ov views that are linked to each other with bi-directiona position constraints. The views within a chian can be distrbuted either vertically or horizontally. 

Chains can be styled in one of the following ways. 
1. Spread: The views are evently distrbuted
2. Spread insdie: The first and last view are affixed to the constraints on each end of the chain and the rest are evenly distributed
3. Weighted: When the chain is set to either spread or spread inside, you can fill the reminaing space by setting one or more views to match constraints `0dp`. By default the space is evenly distributed between each view that's set to match constraints, but you can assign a wieght of importance to each view using the `layout_cocnstraintHorizontal_weight` and `layout_constraintVertical_weight`  attrbitutes. If you're familiar with `layout_weight` in a linear layout, this works the same way. So the view with the highest weight value getes the most amount of space; views that have the same weight get the same amount of of space. 
4. Packed: The views are packed together(after margins). You can then adjust the whole chain's bais by changing the chain's head view bias. 

The chain's head view (the left-most view in a horizontal chain and the top=most view iin a vertical chain) defines the chain's sytle in XML. However you can toggle between spread, spread inside, and packed by selecting any view iin the chain and then clicking the chain button that appears below the view. 

To create a chain, select all of the views to be included in the chain, right-click one of the views select Chains and then select either Center Horizontally or Center vertically. 

Notes
- A view can be a part of both a horizontal and a vertical chain, making it easy to build flexible grid layouts. 
- A chain works properly only if each end of the chain is constrained to another object on the same axis, 
- Althoughthe orientation of a chain is either viertical or horizontal, using one does not align the views in that direction. So be sure you include other constraints to achieve the proper position for each view in the chain such as alignement constraints. 

## Automatically create constrains
Instead of adding constraints to everfy view as you place them in the layout, you can move each view into the positions you desire, and then click Infer Constraints to automatcally create constraints. 

Infer Constraints scans the layout to determine the most effective set of constraints fo all views. It makes a best effort to constrain the views to their current positions while allowing flexibillity. You might need to make some adjustments to be sure the layout reponds as you intend for different screen sizes and orientations. 

Autoconnect to parent is a separate feature that you can enable. If enabled, when you add chilld views to a parent, this feature automtaclly creates two or more constratins fo each view as you add them to the layout but only when its appropriate to constrain the veiw to the parent layout. 

autoconnect is diabled by default. Enable it by clicing Enbale Autoconnection to Parent in the Layout Editor toolbar. 

## KeyFrame animations
Within a ConstraintLayout, you can animate changes to the size and postion of elements by using ConstraintSet and TransitionManager. A `constraintSet` is a lightweight object that represents the constraints, margins, and padding of all child elements within a `constraintLayout`. When you apply a `ConstraintSet` to a displayed `ConstraintLayout`, the layout updates the constraints of all of its children. 

TO build an animation using `ConstraintSet` from teh second keyframe file and apply it to the displayed `ConstraintLayout`.
```
// MainActivity

fun onCreate(savedInstanceState: Bundle?) {
    ...
    constraintLayout = findViewById(R.id.constraint_layout)
}

fun animateTokeyFrameTwo() {
    val constraintSet = ConstraintSet()
    constraintSet.load(this, R.layout.keyframe_two)
    TransitionManager.beginDelayedTransition()
    constraintSet.applyTo(constraintLayout)
}

// layout/keyframe1.xml
// Keyframe 1 contains the starting position for all elements in the animation as well as final colors and text sizes

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/button2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>


```
