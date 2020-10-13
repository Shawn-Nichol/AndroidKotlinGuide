# What is Item Decoration
ItemDecoration allows the application to add a drawing and a layout offset to specific item views from the adapters data set. This is useful for drawing dividers between offsets. 

All ItemDecorations are drawn in the order they were added, before the item views (in onDraw()) and after the items (in onDrawOver(Canvas, RecyclerView, RecyclerView.State). 
