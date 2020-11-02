Adjust the Constraint Bias.
When you add a constaint to both sides of a view, the view becomes centered between the two constraints with a bias of 50% by default. You can adjust he bias by draggin the bias slider in teh attributes window.

## Adjust the view size.
You can use the corner handles to resize a view, but this hard codes the size so the view will not resize for different content or screen sizes. To select a different sizing mode, click a view an dopen the attributres window on the right side of the editor. 

Near the top of the attributes window is the view inspector, which includes controls fo serveral layout attrbiutes. You can change the way the height and widht are calculated by clicking the symbols indicated. 

- Fixed: Specify a apecific dimesnion in the text vox below or by resizing the view in the editor. 

- Wrap Content:  The view expands only as much as needed to fit its contents. 

- Match Content: the view expands only as much as needed to fit its contents. 
  - layout_constraint_default
  - `Spread`: expands the view as much as possible to meet the constraints on each side. This the dfeault behavior. 
  - `Wrap`: expands the view only as much as needed to fit its contents, but still allows  the view to be smaller than that if the constraints require it. 
  
## Size Ratio
You can set the view size to a ratio such as 16:9 if at least one of the view dimenesions is sest to match constraints 0dp. To enable the ratio, click toggle aspect radtio constraints and then enter the width:height ratio in the input that appears. 

If both the width and height are set to match constraints, you can click Toggle aspect reatio constraints to select which dimension is based on a ratioof the other. The view inspector indicates which is sest as a ratio b connecting the corresponding edges with a solid line. 

## Adjust View margins
To ensure that all your views are evenly spaced click Margin 16dp in the toolbar to select the default margin for each view that ou add to the layout. Any change you make to the default margin applies only to the views you add from then on.

You can control the marign for each view in the attributes window by clickign the number on the line that represents each constraint

