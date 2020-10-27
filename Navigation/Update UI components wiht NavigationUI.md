

## Add a Navigation drawer
The navigation drawer is a UI panel that shows your app's main navigation menu. The drawer appears when the user touches the drawer icon in the app bar or when the user swipes a finger from the left edge of the screen. 

The drawer icon i sdisplayed on all top-level destinations that user a Drawerlayout. to add a navigation drawer, first declare a drawerLayout as the root view. inside the DrawerLayout, add a alyout for the main UI content and another view that contains the contents of the navigation drawer. 


```
<?xml version="1.0" encoding="utf-8"?>
<!-- Use DrawerLayout as root container for activity -->
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <!-- Layout to contain contents of main body of screen (drawer will slide over this) -->
    <androidx.fragment.app.FragmentContainerView
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:id="@+id/nav_host_fragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <!-- Container for contents of drawer - use NavigationView to make configuration easier -->
    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true" />

</androidx.drawerlayout.widget.DrawerLayout>
```

Next, connect the DrawerLayout ot your navigation graph by passing it to AppBarConfigrutation.
```
val appBarConfiguration = AppBarConfiguration(navController.graph, drawerLayout)
```

Note when using NavigationUI, the top app bar helpers automatically tansition between the drawer icon and the Up icon as the current destination schagnes. You con't need to use ActionBarDrawerToggle. 

Next, in your main activity class, call setupWithNavController() from your main acivity's oncretae() method

```
override fun onCreate(savedInstanceState: Bundle?) {
    val navHostFragment = supportFragmentManaer.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
    val navController = navHostFragment.navController
    findVieById<NavigationView>(RR.id.nav_view)
        .setupWithNavController(navController)
}
```
    
Note setting up the navigation drawer requires that you also set up your navigation graph and menu xml as descibed in TIe destination to menu items.

## Bottom Navigation
NavigationUI can also handle bottom navigation. When a user selects a menu item, the NavController calls onNavDestinationselected(0 and automatically updates the selected item in the bottom navigation bar. 

To create a bottom navigation bar in your app, first define the bar in your main activity, as shown below. 

```
<LinearLayout>
   ...
   <androidx.fragment.app.FragmentContainerView
   android:id="@+id/nav_host_fragmnet"
   .../>
      <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottom_nav"
        app:menu="@menu/menu_bottom_nav" />
</LinearLayout>
```

Next, in your amin activity class, call setupWithNavController90 from your main activity's onCreate method
```
override fun onCreate(savedInstanceState: Bundle?) {
    setContentView(R.layout.activity_main)

    ...

    val navHostFragment =
        supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
    val navController = navHostFragment.navController
    findViewById<BottomNavigationView>(R.id.bottom_nav)
        .setupWithNavController(navController)
}
```

## List Navigation events
Interacting with the NavController is the primary mehod for navigating betweeen destinations. The NavContoller is responsible for replacing the contents of the NavHost with the new destination.  In many cases, UI elements-such as a top app bar or other persistent navigation controls like a BottomNavigationBar live outside of the NavHost and need to be updated as you navigate between destinations. 

NavController offers an OnDestinationChangedListener interface that is called when the NavController's current destination or its arguemtns change. A new listener can be registereed via the addOnDestinationChangedListener() method Note that when calling addOnDestinationChangedListener(0 method. Note that when calling addOnDestinationChangedListener(0, if the current destination exists, it's immediately sent to your listener. NavigationUI uses OnDestinationChangedListener to make these common UI components navigation-aware. Note, however, that you can also use OnDestinationChagnedListener on its own to make ay  custom UI or buisness lgoic aware of navigation events. 

As ane xapmle, you might have common UI elements that you intend to show in  some areas of your app while hiding them in others. Uisng your own OnDestinationChangedListener, you can aselectively show or hide these UI elements based on the target destination, a s shown in the following example. 

```
navController.addOnDestinationChangedListener { _, destination, _ ->
   if(destination.id == R.id.full_screen_destination) {
       toolbar.visibility = View.GONE
       bottomNavigationView.visibility = View.GONE
   } else {
       toolbar.visibility = View.VISIBLE
       bottomNavigationView.visibility = View.VISIBLE
   }
}
```

