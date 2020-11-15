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
Because the view is yet to load the fragment view cannot be found, for the NavController to be properly set, the fragment view needs to be createded and onViewCreated() needs to be dispateched, which does not happend until the ACTIVITY__CREATED state. 
```
val navHostFragment: NavHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
```

Retrieve the NavController directly from the NavHostFragment
```
val navController = navHostFragment.navController
```

Setup a toolbar for use with a NavController
@ navController: The NavController whose navigation actions will be reflected in the titlle of the Toolbar. 
@ drawerLayout: The DrawerLayout that should be toggled from the Navigation button.
```
myToolbar.setupWithNavController(navController, myDrawerLayout)
```

Set Toolbar as the action bar
```
setSupportActionBar(myToolbar)
```

AppbarConfiguration options for NaviationUI methods that interact with implementaions of app bar patterns
@ topLevelDestinationIds: Set of destinations by id considreed at the top level, the upbutton will not display
@ drawerLayout: The openable layout that should be toggeld from the navigation button. 
```
myAppBar = AppBarConfiguration(setOf(R.id.login_dest, R.id.fragmentMenuOne), myDrawerLayout)
```


Set up the action bar returned by `AppCompatActivity.getSupportActionbar()` for use with navConroller
@navController: The NavController whose navigation actions will be reflected in the title of the action bar. 
@param: Configuration Additional configuration options for customizing the behavior of the  ActionBar
```
setupActionBarWithNavController(navController, myAppBar
```


Heres what it would look like in the main activity. 
```
lateinit var mDrawer: DrawerLayout


class MainActivity : AppCopmatActivity() {

    lateinit var mDrawer: DrawerLayout
    lateinit var myAppBar: AppbarConfiguration
    
    fun onCreate(savedInstanceState: Bundle?) {
      val navHostFragment: NavHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
      val navController = navHostFragment.navController
      myToolbar.setupWithNavController(navController, myDrawerLayout)
      setSupportActionBar(myToolbar)
      myAppBar = AppBarConfiguration(setOf(R.id.login_dest, R.id.fragmentMenuOne), myDrawerLayout)
      setupActionBarWithNavController(navController, myAppBar)
  }
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
