# Creating Menu Groups
A menu group is a collection of menu items that share certain traits. With a group, you can: 

- Show or hide all items wiiht setGroupVIsible()
- Enable or disable all items with setGroupEngabled()
- Specify whether all items are checable with setGroupCheckable()

You can createa a gropu by nesting <item> elements inside a <group> element in your menu resource or by specifying a group ID with the add() method. 

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_save"
          android:icon="@drawable/menu_save"
          android:title="@string/menu_save" />
    <!-- menu group -->
    <group android:id="@+id/group_delete">
        <item android:id="@+id/menu_archive"
              android:title="@string/menu_archive" />
        <item android:id="@+id/menu_delete"
              android:title="@string/menu_delete" />
    </group>
</menu>
```

The tiems that are in the group appear at the same level as the first item all three items in teh menu are sibling however, you can modify the traits of the two items in the group by referencing the group id and using the methods listed above. The system will also never separate grouped items. For example, if you declare android:showAsAction="ifRoom" for each item, they will eithe rboth appear in the action bar or both appear in the action overflow. 

## Using checkable menu items
A menu can be useful as an interface for turning options on and off, using a checkbox for stand-alone options, or radio buttonfs groups of mutally exclusive options. 

Note: Menu items in the Icon Menu cannot display a checkbox or radio button. If you choose to make itmes in the Icon Menu Checkable, you must manually indicate the checked state by swwapping hte cion and or text each tim the state chagnes. 

You can define the checkable behavior for individual men u items using the android:checkable attribute in the <item> element, or for an entire gorup with the android:checkableBehavior attribute in the <group> element. 

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <group android:checkableBehavior="single">
        <item android:id="@+id/red"
              android:title="@string/red" />
        <item android:id="@+id/blue"
              android:title="@string/blue" />
    </group>
</menu>
```

The android: CheckableHevahior attribute accepts either
single:
Only one item from the group can be checked

all: 
All items can be checked

none: 
No items are checkable. 

You can apply a default checked state to an item using the android:checked attribute in teh <item> element and change it in code iwht the setChecked() method. 

When a checkable item is selected, the system calls your respective item-selected callback method. It is here htat you must set the state of the checkbox. becuase a check box or radio button does not change its state automatically. You can query thecurrent state of the item (as it was before the user selected it) with isChecked() and then set the checked state with setChecked()

```
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return when (item.itemId) {
        R.id.vibrate, R.id.dont_vibrate -> {
            item.isChecked = !item.isChecked
            true
        }
        else -> super.onOptionsItemSelected(item)
    }
}
```

If you don't set the check state this way, then the visibl estate of the item will not change when the user selects it. When you do set the state, the acitivty preserves the checked state of the item so that when the user opens the men later, the checked state that you set is visible. \

Note : Chekcable menu items are intended to be used only on a per-session basis and not saved after the application is destoryed. if you have application setting that ou would like to save for the user, you should store the data using SharedPreferences. 

