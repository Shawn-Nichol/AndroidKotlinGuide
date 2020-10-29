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
inner class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view), View.onClickListner {
  ...
    // Initialize the click listener
    init {
    view.setOnClickListner(this)
  }
  
  // Implement onClick method
  override fun onClick(0o: View?) {
    if(adapterPosition != RecyclerView.NO_POSITION) {
      listener.onItemClick(adapterPosition)
    }
  }
}
```




## 3) ADD interface to RVAdapter
Add interface to the bootom of the adapter and the call the function that it will run in the activity or fragment. 

```
interface OnItemClickListener {
  fun onItemClick(position: Int)
}
```

## Activity/Fragment
Implement the interface
```
class MyFragment : Fragment(), RVAdapter.OnItemClickListner {
  
  // This references the listener
  private lateinit var rvAdapter = RVAdapter(this)

  ...
}
```

## 4) Implement Click method
```
Override the method name that was set in the RVAdapter OnItemClickListener
    // the method the interface will call from the RVAdapter. 
    override fun onItemClick(position: Int) {
      ...
    }
}
```
