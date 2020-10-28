# What is a Menu
Menus are a common user inteface componetn in many types of application. To provide a familiar and consistent user experience, you should use the Menu APIs to present user actioins and other options in your activites. 

Beginning with Android 3.0 android powered devices are no longer required to provide a dedicatted Menu button. With this change, android apps should migrate away from a dependence on the traditional 6 item menu panel and instead provide an app bar to present common user actions. 

Although the design and user exeperience fo some menu items have changed, the semantics to define a set of actions and options is still based on the Menu APIs. THis guide shows how to create the three fundamental types of menus or actoins presentations on all version sof android. 



## Option menu and app bar
The Options menu is the primary collection of menu items ofr an activity. It's where you should place actions that have a global impact on the app such as serach compose email and settings

## Context menu and contextual action mode
A context menu is a floating menu that appears when the user performs a long-click on an element. It provides actions that affect the selected content of context frame. 

The contextual action mode displays action items that affect the selected content in a bar at the top of the screen and allows the user to select mutliple items


## Popup menu
A popup menu diplays a list of items in a vertical list that's anchored to the view that invoked the menu. It's good for providing an overflow of actions that relate to specific content or to provide options for a seoncd part of a command. Actioons in a popup menu should not directly affect the corresponding content that's what contextual actions are for. Rather, the popup menu is for extend actions that relate to regions of content in your activity. 


