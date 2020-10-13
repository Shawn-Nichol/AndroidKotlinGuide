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


## Add your own line
Create an xml drawable that define the line you want to use to seperate the items in RecyclerView.

res drawable rv_divider_red.xml
```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <size
        android:width="5dp"
        android:height="10dp" />
    <solid android:color="@android:color/holo_red_dark" />
</shape>
```

Add the drawable to the itemDeocrations.
```
recyclerView.apply {
  adapter = RVADapter(context, this)
  
  var itemDecoration = DividerItemDeocration(this.context, DividerItemDecoration.VERTICAL)
  itemDecoration.setDrawable(getDrawable(R.drawable.rv_divider_red)!!)
  addItemDecoration(itemDecoration)
}
```


