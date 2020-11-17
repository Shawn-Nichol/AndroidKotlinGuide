# What is an App Bar
A primary toolbar within the activity that may display the activity title, application-level navigation and other interactive items. The action bar appears at the top of the activity window when the activity uses the system.

Begining in android API 21 the action bar may be represented by any Toolbar widget within the application layout. The appllication may signal to the activity witch Toolbar should be treated as the Activit's action bar. Activites that use this feature should use one of the supplied .NoActionBar themes, and set the Windows attrbitue to false. 

From your Activity you can retrieve an instance of ActionBar by calling getActionBar()

Key Functions fo the app bar
- A dedicated space for giving your app an identity and indicating the user's location in the app. 
- Access to importatn actions ina predictabl eway, such as search
- Support for navigtaion and view switching.

The most basic form of an action bar displays the title for the activity on one side and overflow menu items on the other. 

The most recent features of app bar are added to the support library's version of Toolbar. Becuase of toolbars backwards compatibility you should use Toolbar.
