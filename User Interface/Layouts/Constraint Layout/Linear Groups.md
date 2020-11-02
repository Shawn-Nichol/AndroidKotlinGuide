## Control Linear groups with a chain. 
A chain is a group of views that are linked to each other with bi-directional position contraints. The views witin a chain cna be distrbuted either vertically or horizontally

Chains have the following styles. 

`Spread`: The views are evenly distributed.

`Spread inside`: The first and last view are affixed tothe constraints on each end of the chain an dthe rest are evenly distributed.

`Weighted`: When the chain is set to either spread or spread inside, you can fill the remaining space by setting one or more views to match constraints `0dp`. By defalt the space is evenly distributed beteween each view that's set to match constraints, but you can assign a wieght of importance to each view uing the 'layout_constraintHorizontal_weight` and `layout_constraintVertical_weight`

`Packed`: The viewa re packed together(after margins). You can then adjust hte whole chians bais by changing the chain's head view bias. 

The chains head view (the left-most view in a horizontal chain and the top-most view in a vertical chain) defines the chains style in XML. However you can toggel beteween spread, spread inside, and packed by selecting any view in the chain an dthen clicking the chain button that appears below the view. 

TO create a view selected al teh views to be included in teh chain, right-click one of the views select Chains and tehn select either center horizontally or cneter vertically.

Notes:
- A view can be a part of both a horizontal and a vertical chain, making it easy to bhild flexible grid layouts.
- A chain works properly only if each end of the chain is constrained to another object on teh same axis.
- although the orientation ofa chain is either vertical or horizontal, using one does not align the view sin that direction. So be sure you include other constraints to achieve the proper positoin for each view in the chain such as alignement constraints. 

