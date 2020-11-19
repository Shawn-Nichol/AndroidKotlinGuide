## Add Dependencies
```
dependencies {
  def nav_version = "2.3.1"

  // Java language implementation
  implementation "androidx.navigation:navigation-fragment:$nav_version"
  implementation "androidx.navigation:navigation-ui:$nav_version"

  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

  // Feature module Support
  implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

  // Testing Navigation
  androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"
}
```


## Create a Nav Graph
Right-click res directory, select New > Android Resource File. The New Resource File dailog appears.


`File name`: nav_graph </br>
`Resource type`: Navigation </br>
`Resource Name`: navigation


## Navigation Editor
In the nav_graph file add the navigation attributes. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            android:id="@+id/nav_graph">

</navigation>
```

## NavHost Fragment
Add View to MainActivity layout.
```
    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"

        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />
```

## Desination
1. In the Navigation Editor, click the new Destination icon and then click create new destination
2. Select fragment for destination, the nav_graph will update. 

## Start Destination
Select the desired destination and click the assign start destination button.

## Connect Destination
Drag the circle on the destination over to the desired destination. 

## Add Action to destination
Click on the destination and add action
- Type: contains action
- ID: ID for the action
- Destination: contains the ID for the destination fragment. 


## Navigate to destination
- Fragment.findNavController()
- View.findNavController()
- activity.findNavController()

```
val navHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
val navController - navHostFragment.navController
findNavHostController().navigate() 
```

## Argument SafeArgs
Add SafeArgs to gradle plugin
```
buildscript {
    repositories {
        google()
    }
    dependencies {
        def nav_version = "2.3.1"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
```

Apply to app gradle file
```
apply plugin: "androidx.navigation.safeargs.kotlin"
```

Add Argument to destination. Build app this will generate two new java classes, the first class will use the layout name with Directions appended to the end the second will name will be the amend args to end.

Fragment One
```
val action = FragmentOneDirections.myargs(myData)
```

Fragment Two
```
val args = FragmentTwo.fromBundle(requireArguments())

val passedData = args.myargument
```
