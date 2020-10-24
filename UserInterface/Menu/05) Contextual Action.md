# Using Contextual action mode
The contextual action modeis system implementation of ActionMode that focuses user interaction toward performing contextual actions. When a user enables this mode by selectingan itme, a contextual action bar appears at the top of the screen to present actioins the user can perfrom on the curretnly selecte item. While this mode is enabled, the user can selecte multiple items, deselect items, and continue to navigate within the activity (as much as you're willing ot allow). The action mode is disabled and the dcontextual action bar disapppears when the user deselects all items, presses the BACK button, or selects the Done action on the left side of the bar. 

For views that provide contextual actions, you should usually invokke the contextual action mode upon one of th two events 

- The users performs a long click
- The user selects a checkbox or similar UI component within the view. 

How your application invokes the contextual actionmode anddefines the behavior for each action depends on your design. There are basically two designs: 
- For contextual actions on indiviual arbitrarry ivews. 
- For batch contextual actions on groups of itmes in a ListView or GridView(allowing the user to select multiple itmes and perform an action on them all). 

Enabling the ctextual action mode for indvidual views. 

If you want to invoke the contextual action mode only when the user selectes specific views. 
1. Implmement the ActionMode.Callback interface. In its callback methods, you can specify the action for the contextual action bar, respond to click events on action items, and handle other lifecycle events for the action mode. 

2. Call startActionMode() when you want to show the bar(such as when the user long-clicks the view)

Implement the ActionMode.Callback interface. 
```
private val actionModeCallback = object : ActionMode.Callback {
    // Called when the action mode is created; startActionMode() was called
    override fun onCreateActionMode(mode: ActionMode, menu: Menu): Boolean {
        // Inflate a menu resource providing context menu items
        val inflater: MenuInflater = mode.menuInflater
        inflater.inflate(R.menu.context_menu, menu)
        return true
    }

    // Called each time the action mode is shown. Always called after onCreateActionMode, but
    // may be called multiple times if the mode is invalidated.
    override fun onPrepareActionMode(mode: ActionMode, menu: Menu): Boolean {
        return false // Return false if nothing is done
    }

    // Called when the user selects a contextual menu item
    override fun onActionItemClicked(mode: ActionMode, item: MenuItem): Boolean {
        return when (item.itemId) {
            R.id.menu_share -> {
                shareCurrentItem()
                mode.finish() // Action picked, so close the CAB
                true
            }
            else -> false
        }
    }

    // Called when the user exits the action mode
    override fun onDestroyActionMode(mode: ActionMode) {
        actionMode = null
    }
}
```

Notice that these event callbacks are almost exactly the same as the callbacks for the options menu, expect each of these also pass the ActionMode object associated with the event. you can use ActionMode APIs to make various changes to the CAB, such as revise the title and subtitle with setTitle() and setSubtitle() useful to indicate how many items are selected. 

Also notice that the above sample sets the mActionMode variable null when the actionmode is destoryed. In the next step, you'll see how its initialized and how saving the member variable in your activity or fragment can be useful 

2. Call startActionMode() to enable the contextual action mode when appropriate, such as in response to a long-click. 

fragment
```
fun myCAB() {
    if(mAction != null) return 
    mActionMode = (Activity as MainActivity?)!!.startSupportActionMode(actionModeCallback)
}
```

When you call startActionMode9), the system returns the ActionMode created. By saving this in a member variable,you can make changes to the contextual action bar in response to other events. In the avobe sample, the Actioinmode is used to ensur ehtat the Actionmode instance is not recreateed if it's already active, by chcking whether the member is null before startin the action mode. 


## Enabling batch contextual action in a ListView or GridView
If you have a collection of items ina ListView or GridView (or another extension of AbsListView0 and want to allow users to perform batch actions, you should
- Implement the AbsListView.MultiChoiceModeListener interface and set it for the view group with setMultiChoiceModelIstener(). In the Listener's callback methods, you can specify the actions for the contextual action bar, respond to click events on actions items and handle other callbacks inherited for the Actionmode.callback interface. 

- Call setChoiceMOde() with the CHOICE_MODE_MULTIPLE_MODAL argument

```
val listView: ListView = getListView()
with(listView) {
    choiceMode = ListView.CHOICE_MODE_MULTIPLE_MODAL
    setMultiChoiceModeListener(object : AbsListView.MultiChoiceModeListener {
        override fun onItemCheckedStateChanged(mode: ActionMode, position: Int,
                                               id: Long, checked: Boolean) {
            // Here you can do something when items are selected/de-selected,
            // such as update the title in the CAB
        }

        override fun onActionItemClicked(mode: ActionMode, item: MenuItem): Boolean {
            // Respond to clicks on the actions in the CAB
            return when (item.itemId) {
                R.id.menu_delete -> {
                    deleteSelectedItems()
                    mode.finish() // Action picked, so close the CAB
                    true
                }
                else -> false
            }
        }

        override fun onCreateActionMode(mode: ActionMode, menu: Menu): Boolean {
            // Inflate the menu for the CAB
            val menuInflater: MenuInflater = mode.menuInflater
            menuInflater.inflate(R.menu.context, menu)
            return true
        }

        override fun onDestroyActionMode(mode: ActionMode) {
            // Here you can make any necessary updates to the activity when
            // the CAB is removed. By default, selected items are deselected/unchecked.
        }

        override fun onPrepareActionMode(mode: ActionMode, menu: Menu): Boolean {
            // Here you can perform updates to the CAB due to
            // an <code><a href="/reference/android/view/ActionMode.html#invalidate()">invalidate()</a></code> request
            return false
        }
    })
}
```

Now when the user selects an item with a long-clic, the system calls the onCreateActionMode() method and displays teh contextual action bar with the specified actions. While the contextual action bar is bisible, users can select additional items. 

In some cases in which the contextual actions provide common actions items, you might want to ad a checkbox or similar UI element that allows users to select items, becuase they might not discover the long-click behavior. When a user selects the checkbox, you can invoke the contextual action mode by setting the respective list item to the checked state with setItemChecked(). 

