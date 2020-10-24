# Handling click events
When the user selects an item from the options menu(including actions items in the app bar), the system calls your activity's onOptionsItemSelected() method. 



## onOptionsItemSelected
This hook is called whenever an item in your options men is slected. The default implementation simply returns false to have the normal processing happen( calling th item's Runnable or sending a message to its Handler as appropriate. 

item: The menu item that was selected. This value cannot be null. 
return Return false to allow normal menu processing to procced, true to consume it here. 

```
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    // Handle item selection
    return when (item.itemId) {
        R.id.new_game -> {
            newGame()
            true
        }
        R.id.help -> {
            showHelp()
            true
        }
        else -> super.onOptionsItemSelected(item)
    }
}
```

When you succeessfully handle a menu item, return true. If you dont' handle the menu item, you should call the superclass implementation of onOptionsItemSelected() the default implementation returns false. 

If your activity includes fragments, the system first calls onOptionsItmeSelected() for the activity then for each fragment until one returns true or all fragments have been called. 

Android 3.0 adds the ability for you to define the on-click behavior for a menu item in XML, using he android:onClickattribute. The value for the attribute must be the name of the method defined by the activity using the menu. The mehtod must be public and accept a single MenuItem paramaeter when the stystem calls this method, it passes the menu item selected. 

If your application contains multiple activites and some of them provide the same optionsMenu. Creating an activity that implements nothing except the onCreateOptionsMenu() and onOptionsItemSeleted() methods. Then extend this class for each activity that should share the same options menu. This way, you can manage one set of code for handling menu actions and each descendant class inherits the mennu behaviors. If you want to add menu items to one of the descendant activities, override onCreatOptionsMenu() in  that activity. Call super.onCreatOptionsMen(menu) so the orignal menu items are created, then add new menu items with menu.add(). you can also override the super class's behavior for indvidual menu items. 

