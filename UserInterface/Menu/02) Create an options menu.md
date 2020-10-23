# Creating an Options Menu
THe options menu is where you should include actions and other options that are relevant to the current activity context, such as "Search" compose email and settings. 

Where the items in your options menu appear on the screen depends on the version for which you've developed your applicaition. 

You can declare items for th eoptions men from either your activity subclass or a fragment subclass. If both your activity and fragments decalre items for the options menu, they are combined in the UI. THe activit's items appear first, followed by those of each fragment in the order in which each fragment is added to the activity. If necessary, you can re-order the menu items with the anroid:orderInCateogry attribute in each <item> you need to move. 

To specify the options menu for an activity, override onCreateOptionsMenu() fragments provide theier own onCreateOptionsMenu() callback. In thsi method you can inflaate your menu resource define in XML into the Menu provided int he callbac. 
```
override fun onCreateOptionsMenu(menu: Menu):Boolean {
  val inflater: MenuInfalter = menuInflater
  inflater.inflate(R.menu.game_menu, menu)
  return true
}
```

You can also add menu items using add() and retrieve items with findItem() to revise thier properites with MenuItems

If you've developed your application for android.2.3.x and lower, the systme calls onCreateOptionsMenu to create the options menu when the user opens the menu for the fist time. ifyou've developed for android 3.0 and higher, the systme calls onCreateOptionsMen9) when starting the activit, in orderr to show items to the app bar. 

## Changing menu items at runtime
After the systme calls onCreateOPtionsMenu(), it retains an instance of the Menu you populate and will not call onCreateOptionsMenu() agian unless the menu is invalidated for some reason. However, you should use onCreateOptionsMenu() only  to create the intital menu state and not to make changes during the activity lifecycle. 

If you want to mdoify the options menu based on events that occur duringin the activity lifecycle, you can do so in teh onPrepareOptionsMenu() method. This method passes you the Menu obect as it currently exists so you can modify it, such as add, remove or disable items. Fragments also provide an opPrepareOptionsmenu() callback. 

On Android 2.3.x and lower, the systme calls onPrepareOptionsMenu() each time the user opns the options menu

On Android 3.0 and higher the options menu is consdiered to always be open when menu items are presented in the app bar. When an event occurs and you wnat to perform a menu update, you must call invalidateOptionsmen() to request that the sytem call opPrepareOptionsMen(). 

Note: You should never change items in teh optioins men based on teh View currently in focus. When in touch mode(when the user is not using a trackball or d-pad), views cannot take focus so you d should nevere use focus as the basis for mdofiying items in teh options menu. If you want to provide menu items that are context-sensitive to a View, use a ContextMenu. 

