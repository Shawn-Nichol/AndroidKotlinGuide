# XML
For all menu types, Android provides a standard XML format to define menu items. Define a menu and all of its items in an XML menu resource. You can then inflate the menu resource in your activity or fragment. 

Using a menu resource is a good practice for a few reasons. 
- It's easier to visualize the menu structure in XML.
- It separates the content for the menu from your application's behavioral code. 
- It allows you to create alternative menu configurations for differenet platform bersions, screen sizes, and other configurations by leveraging the app resources framework. 

To define the menu, create an XML file inside your probject's res/menu// directory and build the menu with the following elements. 

## <menu>
Defines a Menu, which is a continaer ofr a menu items. A <menu> elememtn ust be the root node for th file and can hold one or more <item> and <group> elements. 

## <item>
Creates a MenuItem, which represents a single item in a menu. this element may conain a nested <menu. elememtn in order to dreate a submenu. The <item> element supports several attributes you can use to deinfe an item's appearance and behavior. THe items in the above menu include the following attributes

android:id
A resource ID that's unique to the item, which allows the application to recongize th eitem when the user selects it. 

android:icon
A reference to a drawable to use as the item's icon

android:title
A reference to a stirng to use as the item's title. 

android: showAsAction
Specifies when and how this item should appear as an action item in the app bar. These are the most important attributes  you should use, but there are many more avaiable. For information about all the supported attributes, see the Menu Resource document. 


## <group>
An optional, invisible container for <item>elements. It allows you to categorize menu items so they share properites such as active state and visiblity.

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
You Can add a submenu to an item in any menu by adding a <menu> element as the child of an <item> Submenus are useful when your applicatioin has a lot of functiosn that can be organized into topics, like items in a PC application's menu bar (file, Edit, View, etc.)

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

To use the menu in your activity, you need to inflate the menu resource(convert the XML resource into a programmable object) using MenuIflater.inflate(). 
