# Create a floating context menu
1. Register the View to which the context menu should be associated with by calling registerForContextMenu() and pass it the View. If Your activity uses a ListView or gridVIew and you want each item to provide the same context menu, register all items for a context menu by passing the ListView or GridView to registerForContextMenu(). 
```
registerContextMenu(binding.myView)
```


2. Implement the onCreateContextMenu() method in your Activity or fragment. When the registered view receives a long-click event, the system calls your onCreateContextMenu() method. This is where you define the menu items, usually by inflating a menu resource. 

```
override onCreateContextMenu(menu: ContextMenu, v: View, menuInfo: ContextMenu.ContextMenuInfo) {
  super.onCreateContextMenu(menu, v, menuInfo)
  val infalter = MenuInflater(context)
  infalter.inflate(R.menu.context_menu, menu)
}
```

MenuInflater allows you to inflate the context menu from a menu resource. The callback method parameters include the view that the user selected and ContextMenu.ContextMenuIfno object that provides additional information about the item selected. If your activity has several views that each provide a different context menu, you might use these parameters to determine which context menu to inflate. 

3. Implement onContextItemSeleteced(). When the user selects a menu item, the system calls this method so you can perform the appropriate action. 
```
override fun onContextItemSelected(item: MenuItem): Boolean {
    val info = item.menuInfo as AdapterView.AdapterContextMenuInfo
    return when (item.itemId) {
        R.id.edit -> {
            editNote(info.id)
            true
        }
        R.id.delete -> {
            deleteNote(info.id)
            true
        }
        else -> super.onContextItemSelected(item)
    }
}
```

The getItemId() method queries the ID for the selected menu item, which you should assign to each menu item in XML using the adrioid:id attribute. 

When you successfully handle a menu item, return true. If you don't handle the menu item, you should pass the menu item to the superclass implementation. If your activity includes fragments, the activity receives theis callback first. By calling the superclass when unhandled, the system passes the event to the respective callback method in each fragment , one at a time. implementation for Activity and android.ap.Fragment return false, so you should always call the superclass when undhandled. 
