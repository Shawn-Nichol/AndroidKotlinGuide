#Popup menu
A PopupMenu is modal menu anchored to a view. It appears below the anchor view if there is room or above the view otherswise. It's usefor
- Providing an overflow-style menu for actions that relate to specific content

Note: This not the sma as context menu, which is generally for actions that affect selected content. For action tha affect selected content, use the contextual action mode or floating context menu. 

- Providng a second part of a command sentence (such as a button marked add that produces a popup menu with different add options. 
- Providinga drop-down similar to spinner that does not retain a persisten selection

If you define you menu in XMl, here's how you can show the popup menu: 

1. Instantiate a popupmen with its constructor, which takes the current application context and the view to which the men u should be ancochored
2. Use MenuInflater to infalte your menu resource into the Menu object returned by PeopupMenu.getMenu()
3. Call PopupMenu.show()

```
<ImageButton
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/ic_overflow_holo_dark"
    android:contentDescription="@string/descr_overflow_button"
    android:onClick="showPopup" />
```

Activity
```
fun showPopup(v: View) {
    val popup = PopupMenu(this, v)
    val inflater: MenuInflater = popup.menuInflater
    inflater.inflate(R.menu.actions, popup.menu)
    popup.show()
}
```

## Hanlding click events
To peform an action when the user selects a menu item, you must implement the PopupMenu.OnMenuItemClicListener interface and register it with your PopupMenu by calling setOnMenuItmeclickListner(). When the user selects an item , the systme calls the onMenuItemCLick() clalback in your interface. 

```
fun showMenu(v: View) {
    PopupMenu(this, v).apply {
        // MainActivity implements OnMenuItemClickListener
        setOnMenuItemClickListener(this@MainActivity)
        inflate(R.menu.actions)
        show()
    }
}

override fun onMenuItemClick(item: MenuItem): Boolean {
    return when (item.itemId) {
        R.id.archive -> {
            archive(item)
            true
        }
        R.id.delete -> {
            delete(item)
            true
        }
        else -> false
    }
}
```
