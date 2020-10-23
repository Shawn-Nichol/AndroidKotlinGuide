# Top app bar
The top app bar provides a consisten place along the top of your app for displaying information and actions from the current screen. 

Navigation UI contains methods that automatically update content in yout top app bar as users navigate through your app. For example, NavigationUI uses the destinations labels from your navigation graph to keep the title of the top app bar up-to-date.
```
<navigation>
    <fragment ...
              android:label="Page title">
      ...
    </fragment>
</navigation>
```


When using the NavigationUI with top app bar implementations discussed below, the label you attach to destinations can be automatically populated formthe arugments provided to the destination by using the format of {argName in your lable. 

NavigationUI proivdes upport for the following top app bar types
- Toolbar
- CollapsingToolbarLayout
- ActionBar

## AppBarConfiguration
Navigation uses an AppBarConfiguration object to manage the behavior of the Navigation button in the upper-left corner of your app's display area. The navigation button's behavior changes depending on whether the user is at  a top-level destination. 

 A top-level destinatino is the root, or highest level destination, in a sest of hierarchically-related destinations. Top-level destinations do not display an Up button in the top app bar becuase there is no higher level destination. By default, the start destination of your app is the only top-level destination. 
 
 When the user is at a top-level destination, the Navigationbutton becomes a drawer icon if the destination uses a DrawerLayout. If the destination doesn't use a DrawerLayout, the Navigation button is hidden. When the user is on any other destination, the Navigation button appears as an up button. To configure the Navigation button using only the start destination as the top-level destination, create an AppBarConfiguration object, and pass in the corresponding navigation graph, 
 
 ```
 val appBarConfiguration = AppBarConfiguration(navController.graph)
 ```
 
 ## Create a Toolbar
 To create a toolbar with NavigationUI, first define the bar in your main activity, as shown below. 
 ```
 <LinearLayout>
    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar" />
    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        ... />
    ...
</LinearLayout>
 ```
 
 Next, call setupWithNavController() from your main activity's onCreate() method.
 ```
 override fun onCreate(savedInstanceState: Bundle?) {
    setContentView(R.layout.activity_main)

    ...
    
    val navController = findNavController(R.id.nav_host_fragmnet)
    val appBarConfiguration = AppBarConfiguration(navcontroller.graph)
    findViewById<ToolBar>(R.id.toolbar)
      .setupWithNavController(navController, appBarConfiguration)
 ```
 
 ## Collapsing Toolbar
 ```
 <LinearLayout>
    <com.google.android.material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="@dimen/tall_toolbar_height">

        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar_layout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleGravity="top"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">

            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"/>
        </com.google.android.material.appbar.CollapsingToolbarLayout>
    </com.google.android.material.appbar.AppBarLayout>

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        ... />
    ...
</LinearLayout>
 ```
 
 call setupWithNavController() from your main activity's onCreate method.
 ```
 override fun onCreate(savedInstanceState: Bundle?) {
    setContentView(R.layout.activity_main)

    ...

    val layout = findViewById<CollapsingToolbarLayout>(R.id.collapsing_toolbar_layout)
    val toolbar = findViewById<Toolbar>(R.id.toolbar)
    val navHostFragment =
        supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
    val navController = navHostFragment.navController
    val appBarConfiguration = AppBarConfiguration(navController.graph)
    layout.setupWithNavController(toolbar, navController, appBarConfiguration)
}

## ActionBar
To add navigation support ot hte default action bar, call setupActionBarWithNavController() from your main activity's onCreate() method, as shown below. Note that you need to declare your AppBarConfiguration outside of onCreate(), since you also use it when overriding onSupportNavigateUp()
```
private lateinit var appBarConfiguration: AppBarConfiguration

...

override fun onCreate(savedInstanceState: Bundle?) {
    ...

    val navHostFragment =
        supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
    val navController = navHostFragment.navController
    appBarConfiguration = AppBarConfiguration(navController.graph)
    setupActionBarWithNavController(navController, appBarConfiguration)
}
```
Override onSupportNavigateUp() to handle Up navigation
```
override fun onSupportNavigateUp(): Boolean {
    val navController = findNavController(R.id.nav_host_fragment)
    return navController.navigateUp(appBarConfiguration)
            || super.onSupportNavigateUp()
}
```

## Support App bar variation
Adding the top app bar to your actiivty works welll when the app bar's layout is similar for each destination in your app. If, however you top app bar changes substantially across destinations, then consider removing the top app bar from your activity and defining it in each destination fragment, instead. 

As an example, one of your destinations may use a standard toolbar, while another uses an AppBarLayout to cretae a more comlex app bar with tabs

To implement this examle wihtin your destination fragment using NavigationUI, first define the app bar in each of your fragment layouts, beginning with the destination fragmentthat uses a standard toolbar. 
```
<LinearLayout>
    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        ... />
    ...
</LinearLayout>
```


Next, define the destinatio fragment that uses an app bar with tabs
```
<LinearLayout>
    <com.google.android.material.appbar.AppBarLayout
        ... />

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            ... />

        <com.google.android.material.tabs.TabLayout
            ... />

    </com.google.android.material.appbar.AppBarLayout>
    ...
</LinearLayout>
```

The navigation configuration logic is the same for both of these fragments, except that you should call setupWithNavController() from witin each fragment's onVeiwCreated() methods instead of initializing them from the activity. 

```
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    val navController = findNavController()
    val appBarConfiguration = AppBarConfiguration(navController.graph)

    view.findViewById<Toolbar>(R.id.toolbar)
            .setupWithNavController(navController, appBarConfiguration)
}
```

## Tie destinations to menu items. 
NavigationUI also provides helpers for tying destinations to menu-driven UI components. NavigationUI contains a helpler method, onNavDestinationSelected(), which takes a MenuItme along with the NavContrller that hosts the associated distination.If the id of the MnuItme matches the id of the destination, the NavController can then navigate to that destination. 

As an example, the XML snippets below define a menu item and a destination with a common Id, details_page_fragment
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    ... >

    ...

    <fragment android:id="@+id/details_page_fragment"
         android:label="@string/details"
         android:name="com.example.android.myapp.DetailsFragment" />
</navigation>

<menu xmlns:android="http://schemas.android.com/apk/res/android">

    ...

    <item
        android:id="@+id/details_page_fragment"
        android:icon="@drawable/ic_details"
        android:title="@string/details" />
</menu>
```
If your menu was added via the Activity's onCreteOptionsMenu() for example, you can associate the menu items with destinations by overriding the Activity's onOPtionsItemSelected() to call onNavDestinationSelected(), as shown i nthe following example:
```
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    val navController = findNavController(R.id.nav_host_fragment)
    return item.onNavDestinationSelected(navController) || super.onOptionsItemSelected(item)
}

```

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

