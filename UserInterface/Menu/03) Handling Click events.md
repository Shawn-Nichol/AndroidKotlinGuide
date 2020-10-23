# Handling click events
When the user selects an item from the options menu(including actions items in the app bar), the sysmte calls your activity's onOptionsItemSelected() method. This mehod passes the MenuItem selected. You can identify the item by calling getItemId(), which returns the unique ID for the menu item(Defined by the android:id attribute int he menu resource or with an integer given to the add() method you can match this ID against known menu items to perform the approopriate action

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

When you ssucceessfully handle a menu item, return true. If youdont' handle the menu item, you should call the superclass implementation of onOptionsItemSelected() the default implementation returns false. 

If your activity includes fragments, the systme first calls onOptionsItmeSelected() for the activity then for each fragment until one returns true or all fragments have been called. 

Tip: Android 3.0 adds the ability for you to define the on-click behavior for a menu item in XML, using he android:onClickattribute. The value for the attribute must be the name of the method defined by the activiyt using the menu. The mehtod must be public and accept a single MenuItem paramaeter when the stystem calls this method, it passes the menu item selected. 

Tip; If your application contains mutiiple activites and osme of them provide the same optionsmenu. consider creating an activity that implements notheing except hre onCreateOptionsMenu() and onOptionsItemSeleted(0 emthods. Then extend this calss for each activit that should share the same options menu. THis way, you can manage on eset of code for handling menu actions and each descendant class inherits the mnu behaviors. If you want to add menu items to one of the descendant activities, override onCreatOptionsMen() in  that activity. Call super.onCreatOptionsMen(menu) so the orignal menu items are created, then add new menu items with menu.add(). you can also override the super class's behavior for indvidual menu items. 

