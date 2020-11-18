# ItemTouchHelperCallback
THis class is the contract between ItemTouchHelper and your application. It lets you control which touch behaviors are enabled per each ViewHolder and aslo receive callbacks when user perform these actions. 

To control which actions user can take on each view, you should override getMovementFlags(RecyclerView, ViewHolder and return appropriate set of directions flags. (LEFT, RIGHT, START, END, UP, DOWN). You can use makeMovementFlags(int, int0 to easily construct it.

If user drags an item, ItemTouchHelper will call onMove(recyclerView, dragged, target). upon reciving this callback, you should move th item from the old position dragged.getAdapterPosition() to the new position target.getAdpaterPosition() in your adpater and aslo call RecyclerView.Adapter#notifiyItemMoved(int, int). To control where a View can be dropped, you can override canDropOVer(RecyclerView, ViewHolder, ViewHolder). When a draggin View overlaps multiple other views, Callback chooses the closes View with which you have a custom LayoutManager, you can override choosDropTarget(ViewHolder, to select a custom drop target. 

When a veiw is swiped, ItemTouchHelper animates it until  it goes out of bounds then calls onSwiped(viewHolder, int). At ths point, you should update your adapter and call related Adapter#notify event. 
