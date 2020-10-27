## Add dependency
```
dependencies {
  implementation 'com.google.android.material:material:1.0.0'
}
```

## Setup drawer resources

drawer_menu
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
In order to slide navigation drawer over the ActionBar, you need to use the Toolbar widget

```
<androidx.appcompat.widget.Toolbar
    android:id="@+id/my_toolbar"
    android:layout_height="?attr/actionBarSize"
    android:layout_width="match_parent"
    android:fitsSystemWindows="true"
</androidx.appcompat.widget.Toolbar>
```

To use the toolbar, make sure to be using a theme that doesn't include an action bar. 


## Navigation Layout
<!-- This DrawerLayout has two children at the root  -->
<androidx.drawerlayout.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <!-- This LinearLayout represents the contents of the screen  -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!-- The ActionBar displayed at the top -->
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/my_toolbar"
            android:layout_height="?attr/actionBarSize"
            android:layout_width="match_parent"
            android:fitsSystemWindows="true"
        </androidx.appcompat.widget.Toolbar>

        <!-- The main content view where fragments are loaded -->
        <FrameLayout
            android:id="@+id/flContent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </LinearLayout>

    <!-- The navigation drawer that comes from the left -->
    <!-- Note that `android:layout_gravity` needs to be set to 'start' -->
    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nvView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:background="@android:color/white"
        app:menu="@menu/drawer_view" />
</androidx.drawerlayout.widget.DrawerLayout>

## Setup drawer in the activity
 public class MainActivity extends AppCompatActivity {
    private DrawerLayout mDrawer;
    private Toolbar toolbar;
    private NavigationView nvDrawer;

    // Make sure to be using androidx.appcompat.app.ActionBarDrawerToggle version.
    private ActionBarDrawerToggle drawerToggle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Set a Toolbar to replace the ActionBar.
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        
        // This will display an Up icon (<-), we will replace it with hamburger later
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

        // Find our drawer view
        mDrawer = (DrawerLayout) findViewById(R.id.drawer_layout);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // The action bar home/up action should open or close the drawer.
        switch (item.getItemId()) {
            case android.R.id.home:
                mDrawer.openDrawer(GravityCompat.START);
                return true;
        }

        return super.onOptionsItemSelected(item);
    }
}

##
Navigating between menu items. 
