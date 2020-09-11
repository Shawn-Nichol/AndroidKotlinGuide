# Menu
There are three types of menus

## Options menu and app bar
  - The options menu is the primary collection of menu items for an activity. Its where you should place actions that have a global impact on the app, such as search, Compose and settings
  
## Context menu and contextual actions mode
A context menu is a floating menu that ppaears when the user performs a long-click on element. It provides actions that affect the selected content of context frame. 

The contextual action mode dipslays actions items that affect the selected content in a bar at the top of the screen and allows the user to select multiple itmes. 

## Popup menu
A popup menu displays a list of items in avertical list that's anchored to the view that invoked the menu. It's good for providing an overflow of actions that relate to specific content or to provide options for a second part of a command. Actions in a popup menu should not directly affect the corresponding content that's what contextual actions are for. Rather, the popup menu is for extended actions that relate to regionis of content in your activity 


# Define a menu in XML
For all menu types, androd provides a standard XML format to define menu items. Instead of building a menu in your activity's code, you should define a menu and all its items inan XML menu resource. You can then inflate the menu resource (load it as a Menu object) in your activity or fragment.

Using a menu resource is a good practice for a few reasons
- It's easier to visualize the menu structure in XML
- It separattes the content for the menu from your application's behavioral code
- It allows you to create alternative menu configurations for different platform versions, screen sizes, and other configuartions by leveraging the app resources framework. 

To define the menu, create an XML file inside your projects re/menu directory and build the menu with the following elements. 

## <menu>
Defines a menu, which is acontianer for menu items. A <menu> element must be the root node for the file and can hold one or more <item> and <group> elements.

## <item>
Creates a MenuItem, which represents a single item in a menu. THis element may containa nestee <menu> elememtn in order to create a submenu.

## <group>
An optional, inviisble container for <item> elements. It allows you to categorize menu itmes so they share properties such as active state and visibility. For more information, see the section about Creating Menu Groups. 

Here's an example menu named game_menu.xml
```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/new_game"
          android:icon="@drawable/ic_new_game"
          android:title="@string/new_game"
          android:showAsAction="ifRoom"/>
    <item android:id="@+id/help"
          android:icon="@drawable/ic_help"
          android:title="@string/help" />
</menu>
```

The <item> element support several attributes you can use to define an item's apperance and beahvior. The items in teh above menu include the following attributes
### android:id
A resource ID that's unique to the item, which allows the application to recognize the itme when the user selects it. 

### android:icon
A reference to a drawable to use as the item's icon.

### android:title
A reference to a stirng to use as the item's title.

### android: showAsAction
Specifies when and how this item should appear as an action item in the app bar. 

These are the most important attributes you should use, but there are man y more available. For information about all the supported attributes, see the Menu Resource docuemtn. 

You can add a submenu to an item in any menu by adding a <menu> element as the child of an <item. Submenus are useful when your application has a lot of functions that can be organized into topics, like items in a PC application menu bar. 

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
