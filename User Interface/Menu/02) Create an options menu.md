# Creating an Options Menu
The options menu is where you should include actions and other options that are relevant to the current activity context, such as "Search" compose email and settings. Where the items in your options menu appear on the screen depends on the version for which you've developed your applicaition. 

You can declare items for the options menu from either your activity subclass or a fragment subclass. If both your activity and fragments declare items for the options menu, they are combined in the UI. The activity's items appear first, followed by those of each fragment in the order in which each fragment is added to the activity. If necessary, you can re-order the menu items with the anroid:orderInCateogry attribute in each <item> you need to move. 

To specify the options menu for an activity, 


## onCreateOPtionsMenu
Initialize the contents of the Acitivty's standard options menu. 

menu: The options menu in which you place your items
return: You must return true for the menu to be dispalyed. 
```
override fun onCreateOptionsMenu(menu: Menu):Boolean {
  val inflater: MenuInfalter = menuInflater
  inflater.inflate(R.menu.game_menu, menu)
  return true
}
```

You can also add menu items using add() and retrieve items with findItem() to revise thier properties with MenuItems

Android 2.3.x and lower, the systme calls onCreateOptionsMenu to create the options menu when the user opens the menu for the fist time.
Android 3.0 and higher, the system calls onCreateOptionsMenu() when starting the activity, in order to show items to the app bar. 

## Changing menu items at runtime
After the system calls onCreateOptionsMenu(), it retains an instance of the Menu you populated and will not call onCreateOptionsMenu() again unless the menu is invalidated for some reason. Only use onCreateOptionsMenu()  to create the intital menu state and not to make changes during the activity lifecycle. 

If you want to modify the options menu based on events that occur during the activity lifecycle, you can do so in the onPrepareOptionsMenu() method. This method passes you the Menu object as it currently exists so you can modify it, such as add, remove or disable items. Fragments also provide an opPrepareOptionsmenu() callback. 

Android 2.3.x and lower, the system calls onPrepareOptionsMenu() each time the user opens the options menu.
Android 3.0 and higher the options menu is consdiered to always be open when menu items are presented in the app bar. When an event occurs and you want to perform a menu update, you must call invalidateOptionsMenu() to request that the system call opPrepareOptionsMenu(). 

Note: You should never change items in the optioins menu based on the View currently in focus. When in touch mode(when the user is not using a trackball or d-pad), views cannot take focus so you should never use focus as the basis for modifying items in the options menu. If you want to provide menu items that are context-sensitive to a View, use a ContextMenu. 

