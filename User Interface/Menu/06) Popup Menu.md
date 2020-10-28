# Popup menu
A PopupMenu is modular menu anchored to a view. It appears below the anchor view if there is room or above the view otherswise. It's used for
- Providing an overflow-style menu for actions that relate to specific content

Note: This not the same as context menu, which is generally for actions that affect selected content. For action tha affect selected content, use the contextual action mode or floating context menu. 

- Providng a second part of a command sentence (such as a button marked add that produces a popup menu with different add options. 
- Providinga drop-down similar to spinner that does not retain a persisten selection



1. Instantiate PopupMenu, which takes the current application context and the view to which the menu should be ancochored to. 

Fragment
```
fun myButton(v: View) {
    val popupmenu = PopupMenu(mContext, v)
}
```

2. Use MenuInflater to infalte your menu resource into the Menu object returned by PeopupMenu.getMenu()
```
fun myButton(v: View) {
    val popupmenu = PopUpMneu(mContext, v).apply {
        inflate(R.menu.my_menu)
    }
}
```
3. Call PopupMenu.show()
```
fun myButton(v: View) {
    val popupmenu = PopUpMneu(mContext, v).apply {
        inflate(R.menu.my_menu)
        show()
    }
}
```

## Hanlding click events
To peform an action when the user selects a menu item, you must implement the PopupMenu.OnMenuItemClicListener interface and register it with your PopupMenu by calling setOnMenuItmeclickListner(). When the user selects an item , the systme calls the onMenuItemCLick() clalback in your interface. 

```
MyFragment() : Fragment(), PopupMenu.OnMenuItemClickListener {

    ...
    fun showMenu(v: View) {
        PopupMenu(this, v).apply {
            // MainActivity implements OnMenuItemClickListener
            setOnMenuItemClickListener(this@MyFragment)
            inflate(R.menu.actions)
            show()
        }
    }

    override fun onMenuItemClick(item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.menu_item_one -> {
                ...
                true
            }
            R.id.menu_item_two -> {
                ...
                true
            }
            else -> false
        }
    }
}
```
