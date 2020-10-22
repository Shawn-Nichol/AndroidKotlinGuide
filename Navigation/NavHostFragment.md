## Adding a NavHostFragment via XML
The XML shows NavHostFragment
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.appcompat.widget.Toolbar
        .../>

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"

        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        .../>

</androidx.constraintlayout.widget.ConstraintLayout>

```

Note the following
- android:name attribute contains the class name of your NavHost implementation

- app:navGraph attribute assocaites the NavHostFragment with a navigation graph. The navigation graph specifies all of the destinations in this NavhostFragment to which users can navigate. 

- app:defaultNavHost="true" attrbiute ensures that your NavHostFragment intercepts the system back button. Note that only one NaveHost can be the default. If you have mutltiple hosts in the same layout (two pane laouts, for example), be sure to specify only one default NavHost

You can also use the LayoutEditor to add a NavHostFragment to an activity by doin the following

1) in Your list of project files, double-click on your activity's layout XML file to open it in the layout editor.

2) Within the platte pane, choose the containers cateogry, or alternatively serach for NavHostFragment

3) Drag the NavHostFragment view onto your activity. 

4) Next, in the Navigation Graphs dialog that appears, choose the corresponding navigation graph to associate with this navHostFragment, and then click ok. 




