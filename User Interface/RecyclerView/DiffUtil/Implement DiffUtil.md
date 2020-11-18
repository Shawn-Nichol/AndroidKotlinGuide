# How to Implement DiffUtil in a RecyclerView


## 1) Update RV class Signature
Extend Listadapter with the desire model type and the class that extends the ItemViewHolder, and DiffUtil class. Note you need to import androix library the android library will provide errors.
```
// Extend the ListAdapter
//    @Type of list, RecyclerViewAdapterClass>(
//    @The class that extends ViewHolder(RVAdapter)
//    The DiffUtil class in the Adapter. 
class RVAdapter() : ListAdapter<User, RVAdapter.Viewholder>(MyDiffCallback())
```

## 2) Add the DiffCallback class
Add the call back and implement the following methods

- AreItemsTheSame(): Called by the DiffUtil to decide whether two object represent the same item. 
@oldItem: Position of the old item
@newItem: Position of the new item

@ returns: True if the items respresent the same object or false if they are different. 

- AreContentsTheSame(): Called to check whether two items have the same data. DiffUtil check equality instead of object.equals so that you can change its behavior depending on your UI. 
@oldItem: The position of the old list
@newItem: The position of the new list

@ returns: True if the contents of the items are the same or false if they are different. 
```
class RVADapter() : ListAdapter<User, RVAdapter>(RVAdapter) {
    
    class MyDiffCallback : DiffUtil.ItemCallback<User>() {
        override fun areItemsTheSame(oldItem: User, newItem: User): Boolean {
            // Compare items if false load new list. 
            return oldItem.name == newItem.name
        }

        override fun areContentsTheSame(oldItem: User, newItem: User): Boolean {
            // Compare contents, if false load new list
            return oldItem == newItem
        }
    }

```
## 3) Remove getItem()
DiffUtil will take care of getting the item size so the getItem() can be deleted. 

## 4) update onBindViewHolder
Change reference of the item to getItem(positon)
```
val item = getItem(position)
```

## 5) submit list
In an Activity or Fragment call Adapter.submitList() to pass the list to DiffUtil, a new list will need to be submited in order for the DiffUtil to compare lists. If you use LiveData you post he new list to LiveData and observer will submit the list. 
```
viewModel.usersList?.observe(viewLifeCycleOwner, Observer {
    it?.let {
        Log.i(TAG, "userListObserver, List submitted $it")
        mAdapter.submitList(it)
    }
})
```
