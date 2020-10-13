# How to create a basic Line divider
Use the addItemDecoration() method to add a line decoration. 


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



