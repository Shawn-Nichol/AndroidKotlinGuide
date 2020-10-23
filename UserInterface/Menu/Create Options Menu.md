# Create an Options Menu
There are three steps in order to create an options menu. 
1) Create a menu xml file.
2) Load the option menu with onCreateOptionMenu()
3) Handle option menu clicks()

## 1) XML Menu file
Create an XML menu file in the res/menu dirctory. There are three elements avaialbe

### <menu>
The container for the menu items, the menu item must be the root node for the file and can hold one or more <item> and <group> elements. 

### <item> 
Represents a single Item in a menu. This element may contain nested <menu> elements in order to create a submenu. The <item> element supports several attributes to help define the an items appearance behavior. 

- Android:id
The resource ID that is unique to the item, allows the user the reconize the item when the user selects it. 

- Android:icon
A reference to a drawable. 

- android:title
A reference to a string for the item title

-android:showAsAction
Specifies when and how this item should appear as an action itme in the app bar. 


There are more attribute listed in the docs. 

### <group>
An optional, ivisible container for <item> elements. It allows ou to categorize menu items so they share properties such as active state and visibility


```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/file"
          android:title="@string/file" >
        <!-- "file" submenu -->
        <menu>
            <item android:id="@+id/create_new"
                  android:title="@string/create_new" />
            <item android:id="@+id/open"
                  android:title="@string/open" />
        </menu>
    </item>
</menu>

```

## 2) Load options menu
To specify the options menu for an activity override onCreaeteOptionsMenu() fragments provide their own onCreateOptionsMenu() callback. In this in this mehtod you can inflate your resource  designed in XML into the menu provider. If an Activity and Fragment declare items for the options menu, they are combined in the UI. The activity's items appear first, followed by those of each fragemnt in order in which each fragment is added to the activity. 


onCreateOptionMen is called the first time the options menu is displayed, in order to update the menu call onPrepareOptions(menu). The default implementation populates the menu with the standard system menu items. These are placed in the Menu#CATEGORY_SYSTEM group. 

Activity, 
```
/**
 * menu: the options menu in which ou place your items
 * return(boolen): YOu must return true for the menu to be displayed, if false the menu will not be displayed. 
 */
override fun onCreateOptionsMenu(menu: Menu): Boolean {
  val inflater: MenuInflater = menuInflater
  inflater.inflate(R.menu.my_menu, menu)
  return true
}
```


Fragment
Note: for a fragment to implement a menu you need to setHasOptionsMenu to true. 
```
override fun onCreate(savedInstanceState: Bundle?) {
  setHasOptionMenu(true)
}

/**
 * menu: the optionsm menu in which you place your items.
 * inflater: MenuInflater
 */
override fun onCreateOptionsMenu(menu: Menu, inflater: MenuInflater) {
  inflater.inflate(R.menu.fragment_menu, menu)
  super.onCreateOptionsMenu(menu, inflater)
}
```

#### Update menu items
After the system calls onCreateOptionsMenu(), it retains an instance of the Menu you populated and will not call onCreateOptionsMenu() again. If you want to modify the menu you during the activity lifecycle, you can do so in teh onPrpareOptionsMenu(). This method passes the Menu object as it currently exists so you can modify it, such as add, remove or disable items. 

onPrepareOptionsMenu
This method prepare the screen's standard options menu to be dispalyed. This is called right before the menu is shown, every time.

Android/Fragment
```
override fun onPrepareOptionsMenu(menu: Menu) {
  super.onPrepareOptionsMenu(menu)
  menu.removeItem(R.id.menu_item_one)
  menu.add(null, "menu_item_new, NONE, New Menu)
}
```
## 3) Handle click events
When the user selects an item from the option menu (including action items in the app bar), the system calls your activity's onOptionItemSelected(). This method passes the MenuItem selected. You can identify the item by calling getItemId(), which returns the unique ID for the menu item, you can match the ID against known menu items to perform the appropriate action. 

```
override fun onOptionItemSelected(item: MenuItem): Boolean {
    return when (item.itemId) {
        R.id.item_1 -> {
            methodOne()
            true
        }
        R.id.item_2 -> {
            methodTwo()
            true
        }
        else -> super.onItemSelected(item)
    }
}
```
When you successfully handle a menu item, return true. If you don't handle the menu item, call the superclass implementation of onOptionsItemSelected(), the default implementations returns false. 

If you are using an activity and a fragment, the system first callonOptionItemSelected() for the activity then for each frragment until one returns true or all graments have been called. 

Note: Android 3.0 lets you call onClick in the menu XML file to call a method. 

Tip: If your application contains multiple activites and some of them provide the same options menu, consider creating an activity that implements nothing except the onCreateOptionsMenu() and opOptionsItemSelected(). then extend this class for each activit that should share the same optionis menu. This way, you can manage one set of code for hadling menu actions and each descendant class inherits the menu behaviors. If you want to aadd menu items to one of the desendant activites, override onCreateOptioinsMneu() in that activity. Call super.onCreateOptionsMneu(menu) so the orignal menu items are created, then add new menu items with menu.add(). You can also override the super class's behavior for individual menu items. 


