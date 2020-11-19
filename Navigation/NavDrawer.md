# How to setup a Nav bar
This insruction will show how to setup a NavDrawer that works with a NavGraph, all top level Destination will display the hambuger icon and other Destination will display the Up arrow. 

## Add dependency
```
dependencies {
  implementation 'com.google.android.material:material:1.0.0'
}
```

## Setup the Drawer menu
This the menu that will be displayed in the NavDrawer. 
```
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <group android:checkableBehavior="single">
        <item
            android:id="@+id/nav_first_fragment"
            android:icon="@drawable/ic_one"
            android:title="First" />
        <item
            android:id="@+id/nav_second_fragment"
            android:icon="@drawable/ic_two"
            android:title="Second" />
        <item
            android:id="@+id/nav_third_fragment"
            android:icon="@drawable/ic_three"
            android:title="Third" />
    </group>
</menu>
```
You can create subheaders in groups
```
 <item android:title="Sub items">
        <menu>
            <group android:checkableBehavior="single">
                <item
                    android:icon="@drawable/ic_dashboard"
                    android:title="Sub item 1" />
                <item
                    android:icon="@drawable/ic_forum"
                    android:title="Sub item 2" />
            </group>
        </menu>
    </item>

```

## SetupToolbar
In order to slide navigation drawer over the ActionBar, you need to use the Toolbar widget, add the tool bar widget to the main_activity.xml

```
<androidx.appcompat.widget.Toolbar
    android:id="@+id/my_toolbar"
    android:layout_height="?attr/actionBarSize"
    android:layout_width="match_parent"
    android:fitsSystemWindows="true"
</androidx.appcompat.widget.Toolbar>
```

Note: To use the Toolbar, make sure use a theme that doesn't include an action bar. 


## Navigation Layout

```
<!-- This DrawerLayout has two children at the root  -->
<androidx.drawerlayout.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/myDrawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <!-- This LinearLayout represents the contents of the screen  -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!-- The ActionBar displayed at the top -->
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/myToolbar"
            android:layout_height="?attr/actionBarSize"
            android:layout_width="match_parent"
            android:fitsSystemWindows="true"
        </androidx.appcompat.widget.Toolbar>

      
        ... 
        
    </LinearLayout>

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/myNavView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:background="@android:color/white"
        app:menu="@menu/drawer_view" />
</androidx.drawerlayout.widget.DrawerLayout>

```
Note: `android:layout_gravity` needs to be set to 'start'

## Setup drawer in the MainActivity.
The fragment view needs to be createded and onViewCreated() needs to be dispateched, which does not happend until the ACTIVITY__CREATED state. NavHostFragment, swaps different fragment destinations in and out as you navigate throug the navigation graph.
```
val navHostFragment: NavHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
```

NavController, triggers the gragment swap in the NavHostFragment.
```
val navController = navHostFragment.navController
```

DrawerLayout
```
drawerLayout = findViewById(R.id.my_drawer_layout)
```

Set Toolbar
```
val toolbar = findViewById<Toolbar>(R.id.myToolbar)
setSupportActionBar(toolbar)
```

Pass top level destination, shows Nav drawer on the following fragments
Tells the action bar what fragments are top level, and whether the bar must handle a drawer layout.
```
myAppBar = AppBarConfiguration(
    setOf(
        R.id.dest_loginFragment,
        R.id.dest_menu1Fragment,
        R.id.dest_menu2Fragment
    ),
    drawerLayout
)
```

Shows a title based off of the destinations label
Display the UP button whenever you'r not on a top-level destination.
Displays the DrawerIcon when you're on a top-level destination
```
setupActionBarWithNavController(navController, myAppBar)
```


Heres what it would look like in the main activity. 
```
    private fun initNavDrawer() {
        val navHostFragment =
            supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment


        navController = navHostFragment.navController
        drawerLayout = findViewById(R.id.my_drawer_layout)

        val toolbar = findViewById<Toolbar>(R.id.myToolbar)
        setSupportActionBar(toolbar)

        myAppBar = AppBarConfiguration(
            setOf(
                R.id.dest_loginFragment,
                R.id.dest_menu1Fragment,
                R.id.dest_menu2Fragment
            ),
            drawerLayout
        )
        setupActionBarWithNavController(navController, myAppBar)
    }

```


## Handle Clicks
```
myNavView.setNavigationIemtSelectedListener { menuItem -> 
  when(menuItem.itemId) {
  R.id.nav_one
    ...
    true
  }
  R.id.nav_two
    ...
    true
  }
  else -> false
```

## onSupportNaviagte() Handles Up button clicks.
This method is called when ever the user choose to navigate up within the applications hierarchy.
  
@ return: true if Up navigation completed successfully and this Activity was finished, false otherwise.

```
override fun onSupportNavigateUp(): Boolean {
    return findNavController(R.id.nav_host_fragment).navigateUp(myAppBar)
}
```
