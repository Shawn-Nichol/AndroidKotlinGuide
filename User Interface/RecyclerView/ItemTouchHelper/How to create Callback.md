# How to Create a ItemTouchHelper callback

## Create a new class
Create a class that has the following arguments, context and viewModel. Extendd the class with ItemTouchHelper.Callback()

```
class CustomItemTouchHelper(mContext: Context, viewModel: ViewModel) : ItemTouchHelper.Callback() {

}
```

## Implement methods
You will need to implement the following methods, getMovemntFlags(), onMove(), onSwiped(), isItemViewSwipeEnabled(), isLongPRessDragEnabled()
```
class CustomItemTouchHelper(mContext: Context, viewModel) : ItemTouchHelper.Callback() {

  override fun getMovmentFalgs(...) {
    // See instruction below
  }
  
  override fun onMove(...) {
    // See instruction below. 
  }
  
  override fun onSwiped(...) {
    // See instruction below. 
  }
  
  override isItemViewSwipeEnabled(): Boolean {
    // Allows items to be swiped. 
    return true
  }
  
  override isLongPRessDragEnabled(): Boolean {
    // Allows items to be draged after a long click
    return true
  }
  
  override onSelectedChanged(...) {
    // See instruction below, not mandatory. 
  }

}
```

## getMovementFlags()
Sets the flags so you can tell if an item was siped or dragged. 
```
override fun getMovementFlags(
    recyclerView: RecyclerView,
    viewHolder: RecyclerView.ViewHolder
): Int {
    val  dragFlags = ItemTouchHelper.UP or ItemTouchHelper.DOWN
    val swipeFlags = ItemTouchHelper.LEFT or ItemTouchHelper.RIGHT
    return makeMovementFlags(dragFlags, swipeFlags)
}
```


## onMove()
Notifies the adapter of the position the item was dragged to and updates the RecyclerView. 

```
override fun onMove(
    recyclerView: RecyclerView,
    viewHolder: RecyclerView.ViewHolder,
    target: RecyclerView.ViewHolder
): Boolean {
    val startPosition = viewHolder.adapterPosition
    val endPosition = target.adapterPosition
    recyclerView.adapter?.notifyItemMoved(startPosition, endPosition)
    return true
}

```

## onSwiped()
Decideds what to if an item was swiped off screen, you can different results for left and right swipes. 
```
override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
    val editList: MutableList<User>? = viewModel.usersList?.value as MutableList<User>?
    val editListCopy = editList?.toMutableList()
    editListCopy?.removeAt(viewHolder.adapterPosition)
    viewModel.usersList?.postValue(editListCopy)
```

## onSelectedChanged
When an item is selected this will update the background color, this is useful in letting the user now they've selected and item. 
```
override fun onSelectedChanged(viewHolder: RecyclerView.ViewHolder?, actionState: Int) {
    super.onSelectedChanged(viewHolder, actionState)
    if(actionState == ItemTouchHelper.ACTION_STATE_DRAG) {
        viewHolder?.itemView?.setBackgroundColor(Color.LTGRAY)
    }
}
```
