# Creating Contextual menus
A contextual menu offers actions that affect a specific item or context frrame in the UI. You can provide a context menu for any view, but they are most often used for itemes in the ListView, GridView, or other view collectiosn in which the user can perform direct actions on each item. 

There are two ways to provide contextual aactions

- In a floating context menu.  A menu appears asa a floating list of menu items whe the user performs a long-click on a view that ceclars support for a context menu. Users can perform a contextual action on one item at a time. 

- In the contextual action mode. this mode is a system implementation of ActionMdoe that displays a contextual action bar at the top of the screen with actions items that affect the selected items when this mode is active, users can perform an action on multiple items at once. 

Note: The contextual action mode is avaiable on Android3.0 and higher and is the preferred techinque for displaying contextual actions when avaiable. If yor app supports version lower than 3.0 then you should fall back to a floating context menu on those devices. 

## Create a floating context menu
1. Register the View to which the context menu should be associated by calling registerForContextMenu() and pass it the View. if YOur activity uses a ListView or gridVIew and you wwant each item to provide the same context emnu, register all items for a context menu by passing the ListView or GridView to registerForContextMenu(). 

2. Implement the onCretaecontextMenu() method in your Activity or fragment. When the registered view receives a long-click evetn, the system calls your onCreateContextMenu() method. This is where you define the menu items, usually by inflating a menu resource. 

```
override onCreateContextMenu(menu: ContextMenu, v: View, menuInfo: ContextMenu.ContextMenuInfo) {
  super.onCreateContextMenu(menu, v, menuInfo)
  val infalter: MenuInflater = menuInflater
  infalter.inflate(R.menu.context_menu, menu)
}
```

MenuInfalter allows you to inflate the context menu from a menu resource. The callback method parameters include the view that the user selected and ContextMenu.ContextMenuIfno object hat provides additional information about the itemselected. If youactivity has several views that each provide a different context menu, you migh use these parameters to determine hwich context menu to inflate. 

3. Implement onContextItemSeleteced(). When the user selectes a menu item, the system calls this method so you can perform the appropriate action. 
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

The getItemId90 method queries the ID for the selected menu item, which you should assign to each menu item in XML using the adrioid:id attribute. 

When you successfully handle a menu item, return true. If youdon't handle th emenu item, you should pass the menu item to the superclass implementation. If you activity includes fragments, the activity receives theis callback first. By calling the superclass when unhandled, the system passes the vent to the respective callback method in each fragment , one at a time implementation for Activity and android.ap.Fragment retun false, so you should always call teh superclass when undhandled. 

## Using teh contextual action mode. 
