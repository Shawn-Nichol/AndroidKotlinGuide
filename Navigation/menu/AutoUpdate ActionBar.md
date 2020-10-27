

## Set a toolbar in the XML layout
```
    <androidx.appcompat.widget.Toolbar
        android:id="@+id/my_toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary"
        android:elevation="4dp"
        android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
```

# Main Activity 
In the onCreate() method set the Toolbar as the action bar, retrive the navController and post teh NavController on a handler. 


### 1) set the toolbar
```
onCreate(...) {
  setSupportActionBar(findViewById(R.id.my_toolbar)
}
```

### 2) Becuase the view isn't loaded the fragment view cannot be found, for the navcontrller to be properly set, the fragment view needs to be created and onViewCreated() needs to be dispatched, which does not happen until the ACTIVITY_CREATED state. 
```
val navhostFragment: Fragmnet? = supportFragmentManager.frindFragmentById(R.id.nav_host_fragment)
```

### 3)Post the call to the findNavController method on a ahnalder and execute all actions needed. 
```
val navController = navHostFragment!!.findNavController()
```


### 4) SeuptActoinBarWithNavController
Setup the actionBar returned by AppCompatActivity.getSupportActionBar() for use with a NavController. By calling this the title of the action bar will be automatically updated when the destination changes. 

The start destination of your navigation graph is considered the only top level destination. On all other destinations, the ActionBar will show the Up button. Call NavController.navigateup.  to handle the Up button

Activity: AppCompatActivity, the Activity hosting the action bar that should be kept in sync with changes to the NavController. 

NavController: The NavController that supplies the second menu. navigation actions on this navController will be reflected in teh title of the action bar. 
```
navigationUI.setupactionBarWithNavController(this, navController)
```


### onCreate will end up looking like this
```
onCreate(...) {
  ...
  setSupportActionBar(findViewById(R.id.my_toolbar)
  val navHostFragemnt = supportfragmentManager.findFragmentById(R.id.nav_host_fragment)
  val navController = navHostFragment!!.findNavController
  NavigationUI.setupActionBarWithNavController(this, navController)
}
```
