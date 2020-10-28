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

Retrieve the NavController directly forn the NavHostFragment
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

Set up the action bar returned by `AppCompatActivity.getSupportActionbar()` for use with navConroller
@navController: The NavController whose navigation actions will be reflected in the title of the action bar. 
@param: Configuration Additional configuration options for customizing the behavior of the  ActionBar
```
setupActionBarWithNavController(navController, myAppBar
```


## Hanldine Clicks
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
