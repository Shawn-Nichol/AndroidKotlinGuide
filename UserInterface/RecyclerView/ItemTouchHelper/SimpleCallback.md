# ItemTouchHelperCallback
A simple wrapper to the default callback that allows you to swipe and move items in the RecyclerView. 


In the Activity or Fragment unerder the RecyclerView implementation add ItemTouchHelper.SimpleCallback
```
ItemTouchHelper(object: ItemTouchHelper.SimpleCallback(
  ItemTouchHelper.UP or ItemTouchHelper.DOWN, ItemTouchHelper.RIGHT or ItemTouchHelper.LEFT
) {
  /**
   * Called when ItemTouchHelper wants to move the dragged item form its old position to the new position.
   * If this method returns true, ItemTouchHelper assumes viewHolder has been moved to the
   * adapter position of target ViewHolder
   *
   * - recyclerView: The recyclerView to which ItemTouchHelper is attached to.
   * - ViewHolder: The ViewHolder which is being dragged by the user
   * - The ViewHolder over which the currently active item is being dragged.
   */
  override fun onMove(
      recyclerView: RecyclerView,
      viewHolder: ViewHolder,
      target: ViewHolder
  ): Boolean {
      val fromPosition = viewHolder.adapterPosition
      val toPosition = target.adapterPosition

      Log.i(TAG, "fromPosition: $fromPosition, toPosition: $toPosition")

      recyclerView.adapter?.notifyItemMoved(fromPosition, toPosition)

      return true
  }

  /**
   * Called when a ViewHolder is swiped by the user.
   * If you are returning relative directions from the getMovementFlags(recyclerView, ViewHolder)
   * method, this method will also use relative directions. Otherwise, it will use absolute directions
   *
   * ItemTouchHelper will keep a reference to the view until it is detached from RecyclerView. As
   * soon as it is detached, ItemTouchHelper will call clearView(RecyclerView, ViewHolder)
   * - ViewHolder: The ViewHolder which has been siped by the user.
   * - direction: The direction to which the ViewHolder is swiped
   */
  override fun onSwiped(viewHolder: ViewHolder, direction: Int) {
      val editList: MutableList<User>? = viewModel.usersList?.value as MutableList<User>?
      val editListCopy = editList?.toMutableList()
      editListCopy?.removeAt(viewHolder.adapterPosition)
      viewModel.usersList?.postValue(editListCopy)
  }
}

```
