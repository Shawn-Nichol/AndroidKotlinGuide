# How to create a basic RecyclerView. 

## 1) Add dependincies
https://developer.android.com/jetpack/androidx/releases/recyclerview
```
dependencies {
    ...
    // RecyclerView
    implementation "androidx.recyclerview:recyclerview:1.1.0"

}

```
## 2) Create A container 
A container is where is the View that will display RecyclerView in an Activity or Fragment. A container is created to the XML layout file

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        app:layoutManager="LinearLayoutManager"
        tools:listitem="@layout/list_item"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```
To reduce code in the Activity or Fragment you can asign the layout manager in the container. 
Tools: listItem will populate the preview window with the listItem, now you can preview your recyclerview. 

## 3) Create item display
The item display is an xml file saved in layout, it is the view design for each item in the recyclerview. 
```
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/item_title"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

## 4) Create Adapter
Adapters provide a binding from an app-specific data set to views that are displayed within a RecyclerView.

### 4.a) create an class
```
class RecyclerViewAdapter {

}
```

### 4.b) ViewHolder class
A viewholder describes an item view and the metadata about its place within the RecyclerView.  Create a inner class that takes View as parameter and Exteneds RecyclerView.ViewHolder
```
class ItemViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
  val tv: TextView = view.findViewById(R.id.item_title)
  val tv: TextView = view.findViewById(R.id.item_info)
}
```
### 4.c) Update Adapter class signature
Add the parameters context and data set parameters to constructor of the adapter class, and extend the ViewHolder class from the RecyclerView.adapter.
```
class RecyclerViewAdapter(
  private val context: Context,
  private val dataSet: List<Int>
) : RecyclerView.Adapter<RecyclerViewAdapter.ItemViewHolder>() {

...
}
```

### 4.d) Implement required methods
An error will appear you need to implemnt the following methods onCreateViewHolder, getItemCount, onBindViewHolder, this can be done by pressing alt I and selecting all the methods. 

onCreateViewHolder is called by the layout manager to create a new ViewHolder, ViewHolder represents a single item in the dataSet. 
- parent: the ViewGroup into which the new View will be added as a child.
- viewType: the view type of the new view. 
```
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ItemViewHolder {
  val adapterLayout = LayoutInflater.from(parent.context)
    .infalte(R.layout.list_item, parent, false)
    return ItemViewHolder(adpaterLayout)
}
```

getItemCount returns the size of the data set
```
override fun getItemCount(): Int {
  return dataSet.size
}
```

onBindViewHolder, updates the contents of the RecyclerViewHolder.itemView to reflect the item at the given position. 
- holder: the ViewHolder which should be updated to represent the contents of the item at the given position in the data set. 
- position: the position of the item in the adapters data set. 
```
override fun onBindViewHolder(holder: ItemViewHolder, position: Int) {
  val item = dataSet[position]
  holder.tv.text = item.toString()
  // Image can be displayed with the following
  // holder.image.setImageResource(R.drawable.mydrawable)
}
```

## 5) Implement RecyclerView
In the Activity or Fragment that displays the RecyclerView create rv object and instanate the adapter. 
```

val recyclerViewAdapter = RecyclerViewAdapter(this)
override fun onCreate(savedInstanceState: Bundle?) {
  ...
  // Create a dummy list
  val list: MutableList<int> = mutableListOf()
  for(i in 0..100) {
    list.add(i)
  }
  
  val recyclerView = findViewById<RecyclerView>(R.id.recycler_view)
  rv.apply {
    adapter = recyclerViewAdapter
    layoutManager = LinearLayoutManager(baseContext, LinearLayoutManager.Vertical false)
  }
  
  
  recyclerViewAdapter.setList(list)
}
```

include an internal function in the adapter to pass the list to
```
    internal fun setList(player: List<Player>) {
        dataSet = player
        notifyDataSetChanged()
    }
```

