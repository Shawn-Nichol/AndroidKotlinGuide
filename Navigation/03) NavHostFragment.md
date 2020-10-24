## NavHostFragment
NavHostFragment is the continer which Navigation will be using to swap in out destinations, the NavHostFragment should be added to your main_acitivity_layout.



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

```

Note the following
- android:name attribute contains the class name of your NavHost implementation

- app:navGraph attribute assocaites the NavHostFragment with a navigation graph. The navigation graph specifies all of the destinations in this NavhostFragment to which users can navigate. 

- app:defaultNavHost="true" attrbiute ensures that your NavHostFragment intercepts the system back button. Note that only one NaveHost can be the default. If you have mutltiple hosts in the same layout (two pane laouts, for example), be sure to specify only one default NavHost





