# What is Item Decoration
ItemDecoration allows the application to add a drawing and a layout offset to specific item views from the adapters data set. This is useful for drawing dividers between offsets. 

All ItemDecorations are drawn in the order they were added, before the item views (in onDraw()) and after the items (in onDrawOver(Canvas, RecyclerView, RecyclerView.State). 


## How to create a basic Line divider

additemDecoration: Add RecyclerView.ItemDeocration to this RecyclereView.ItemDecoration. Item decoartions are ordered. Decorations placed earlier in the list will be run
queired/drawn first for their effects on item views. Padding added ot views will be nested a padding added by an earlier decoration wil mean further item decoartion in th e list will be asked draw pad within the previos decoration given area. 

DividerItemDecoration: creates a divider ReccylerView.ItemDecoration that can be used with a LinearLayoutManager
- context: Current context, it will be used to accesess resources. 
- orientation: int Divider orientation. Horizontal or Veritcal
```
val rv = findViewById<RecyclerView>(R.id.recycler_view)
rv.apply {
  adapter = RVADapter(context, list)
  addItemDecoration(DividerItemDeocration(this.context, DividerItemDeocration.vertical 
 
}
```


## Custom line


```
recyclerView.apply {
  adapter = RVADapter(context, this)
  
  var itemDecoration = DividerItemDeocration(this.context, DividerItemDecoration.VERTICAL)
  itemDecoration.setDrawable(getDrawable(R.drawable.rv_divider)!!)
  addItemDecoration(itemDecoration)
}
```
