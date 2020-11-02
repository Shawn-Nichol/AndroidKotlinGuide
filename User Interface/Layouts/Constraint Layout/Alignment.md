# Aligment
Align the dege of a view to the same dege of another view. You can offset the alighemtn by dragging the view inward from the constraint. YOu can also select all the view ou want to align and then click `Align` in the toolbar to select the alignemt type. 

## Base Alignment
Align the text baseline of a view to the text baseline of another view. To create a baseline constraint, right-click the text view you want to constrain an then click `Show Baseline`. Then click the text baseline and drag the line to another baseline. 

## Constraint to a Guideline
You can add a vertical or hotizontal guideline to which you can constain views, and the guideline will be invisible to app users. you can position the guideline within the layout based on either dp units or percentage, realtive to the layout's edge. 

To create a guideline, click Guidelines in the toolbar and then click either add vertical Guideline or add horizontal Guideline. Drag the dotted line to treposition it and click the circle at the edge ot he guideline to toggle the mearuement mode. 

## Constraint to a Barrier. 
Similar to a Guideling a Barrier is an invisble line that you can constrain views to. A barrier does not define its own positiono; instead the barrier position moves based on the psoition of views contained within it. 

1. Click Guidelines in the toolbar and then click add vertical/horizontal barrier.
2. In the componet tree window select the views you want  inside the barrier and drag them tinot the barrier component.
3. Select the barrier from the component tree, open the attributres windows, and then stet the barrier direction.

Note: You can include `GuideLine` inside a `Barrier`.

## ADjust the constraint bais. 
