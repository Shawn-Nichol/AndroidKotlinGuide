# Setup App bar

## Select Theme
The app theme shouldn't contain an action bar. 
```
<application
    android:theme="@style/Theme.AppCompat.Light.NoActionBar"
    />
```

## Add Toolbar to layout
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

?attr/actionBarSize: is the height of the action bar. 


## SetSupportActionBar()
Set a Toolbar to act as the ActionBar for this window Activity. The Toolbar's menu will be populated with the Activit's options menu and the navigation button wil be wired through the standard home menu selection action. 

toolbar: Toolbar to set as the Activity's action bar, or null to be clear. 

```
setSupportActionBar(findViewById(R.id.my_toolbar))
```
