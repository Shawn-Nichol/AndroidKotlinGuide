# How to add Click Listener to the RecyclerView


## 1) Add OnItemClickListener to RVAdapter
Add interface to the adapter constructor.
```
class RVAdapter(private val listener: OnItemClickListner) : ListAdapter<User, RVAdapter.ViewHolder>(MyDiffCallback()) {
  ...
}
```

## 2) Update ViewHolder class
Make the class an inner class, and extend View.onClickListner
```
inner class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view), 
  View.onClickListner
```

Initialize the the click listner
```
init {
  view.setOnClickListner(this)
}
```
implement the mehtods required for View.onClickListener
```
override fun onClick(0o: View?) {
  if(adapterPosition != RecyclerView.NO_POSITION) {
    listener.onItemClick(adapterPosition)
  }
}
```
THe ViewHolder class will look this
```
inner class ItemViewHolder(private val view: View) : RecyclerView.ViewHolder(view),
  View.OnClickListener {
  
    ...
    
    init {
      view.setOnClickListener(this)
    }
    
            override fun onClick(p0: View?) {
            if(adapterPosition != RecyclerView.NO_POSITION) {
                listener.onItemClick(adapterPosition)
            }
}
```


## 3) ADD interface to RVAdapter
Add interface to the bootom of the adapter and the call the function that it will run in the activity or fragment. 
```
interface OnItemClickListener {
  fun onItemClick(position: Int)

}

## Implement the interface in the Activity or Fragment
class MyFragment : Fragment(), RVAdapter.OnItemClickListner {
  
  private lateinit var rvAdapter = RVAdapter
  ...
  
  override fun onCreateView( ... ) {
    
      rvAdapter = RVAdapter(this)
  }
}

```

## 4) Implement Click method
Override the method name that was set in the RVAdapter OnItemClickListener
    // the method the interface will call from the RVAdapter. 
    override fun onItemClick(position: Int) {
      
      ...
    }
}
```
