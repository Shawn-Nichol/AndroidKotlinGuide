
```
// add interface to the Adapter constructor
class RVAdapter(private val listener: OnItemClickListener) : ListAdapter<User, RVAdapter.VIewHolder>(MyDiffCallback()) {

  ... 
  // Add inner to the front of the ViewHolder class. 
  inner class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view),
    view.OnClickListener {
    
        inner class ViewHolder(private val view: View) : RecyclerView.ViewHolder(view),
        View.OnClickListener {
        val userName: TextView = view.findViewById(R.id.item_name)
        val userAlbum: TextView = view.findViewById(R.id.item_album)
        var userImage: ImageView = view.findViewById(R.id.item_image)

        init {
            // Initialize the clicklistener on the view
            view.setOnClickListener(this)
        }

        // You could run the click here, but the adapter should only be responsible for the RecyclerView actions. 
        override fun onClick(v: View?) {
            Log.i(TAG, "viewHolder onClick")
            if(adapterPosition != RecyclerView.NO_POSITION) {
                listener.onItemClick(adapterPosition)
            }
        }
    }
    
    ...
    
    // Add interface to the bottom of the adapter and the function that it will run in the activity or fragment. 
    interface OnItemClickListener {
      fun onItemClick(position: Int)

    }
}

// Implement the interface in the RecyclerView. 
class MainActivity : AppCompatActivity()((, RVAdapter.OnItemClickListener {
  ...
  
    // the method the interface will call from the RVAdapter. 
    override fun onItemClick(position: Int) {
      val editList: MutableList<User>? = viewModel.usersList?.value as MutableList<User>?
      Log.i(TAG, "position clicked${editList?.get(position)}")
    }
}
```
