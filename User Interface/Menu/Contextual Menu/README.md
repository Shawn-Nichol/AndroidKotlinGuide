# Creating Contextual menus

A contextual menu offers actions that affect a specific item or context frame in the UI. You can provide a context menu for any view, but they are most often used for itemes in the ListView, GridView, or other view collectiosn in which the user can perform direct actions on each item. 

There are two ways to provide contextual actions

1. In a floating context menu.  A menu appears as a floating list of menu items when the user performs a long-click on a view that declars support for a context menu. Users can perform a contextual action on one item at a time. 

2. In the contextual action mode. this mode is a system implementation of ActionMdoe that displays a contextual action bar at the top of the screen with actions items that affect the selected items when this mode is active, users can perform an action on multiple items at once. 

Android3.0 Contextual ActionMode, is the preferred techinque for displaying contextual actions when avaiable. If your app supports version lower than 3.0 then you should fall back to a floating context menu on those devices. 
