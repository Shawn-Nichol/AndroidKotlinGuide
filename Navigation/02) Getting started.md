## add Dependencies

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

## Create a Navigation graph
Navigation occurs between your app's destinations that is anywhere in your app to which users can navigate. These destinations are connect via actions. 

NavGraph shows a visual representation of a nvigation graph for a sample app contianing six destination connected by five actions. Each destination is represented by a preview thumbnail, and connecting actions are represented by arrows that show how users can navigate form one destination to another. 

Destination: Are differen contents areas in your app
Actions: are logical connections between your destinations that represent paths that users can take. 

To add navigation graph to your project do the following.
1. In the progjec tiwndow, right-click on the res directory and select New > Android Resource File. The New Resource File dailog appears

2. Type a name in the File name fiels, such as nav_graph

When you add your first navigation graph, android studio creates a navigation resource directory within the res directory. This directory contains your navigation graph resource file


## Navigation Editor. 
After adding a graph, Android stuido opens the graph in the navigation Editor. In the Navigation Editor, you can viually edit naviagtion graphs or directly edit the underlying XML.

Destination panel: Lists your navigation host and all destinations currently in the Graph Editor. 

Graph Editor Contains a visual representation of your navigation graph. You can switch between Design view and underlying XML representation in the TextView. 

Attributes: Shows attributes for the currently-selected item in the navigation graph. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            android:id="@+id/nav_graph">

</navigation>
```

The navigation element i sthe root element of a navigation graph. As you add destinations and connecting action to your graph, you can see the corresponding <destination> and <action> elements here as child elements. If you have nested graphs, they appear as child <navigation> elements. 

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
- The android: name attribute contains the class name of your NavHost implementation
- The app:navGraph attribute assocaites the NavHostFragment with a navigation graph. The navigation graph specifies all of the destinations in this NavhostFragment to which users can navigate. 
- Teh app:defaultNavHost="true" attrbiute ensures that your NavHostFragment intercepts the system back button. Note that only one NaveHost can be the default. If you have mutltiple hosts in the same layout (two pane laouts, for example), be sure to specify only one default NavHost

You can also use the LayoutEditor to add a NavHostFragment to an activity by doin the following

1) in Your list of project files, double-click on your activity's layout XML file to open it in the layout editor.

2) Within the platte pane, choose the containers cateogry, or alternatively serach for NavHostFragment

3) Drag the NavHostFragment view onto your activity. 

4) Next, in the Navigation Graphs dialog that appears, choose the corresponding navigation graph to associate with this navHostFragment, and then click ok. 

## Add destinations to the navigation graph. 

you can create a destination from an existing fragment or activity. You can also use the Navigation Editor to create a new destination or create a placeholder to later replace with a fragment or activity 

In this example, lets create a new destination. To add a new destinatnion using the navigatio nEditor. do the following. 

1) In the Navigation Editor, click the New Destination icon and then clidk create new destination. 
2) In the New Android component dialog that appears, create your fragment.

Back  int he Navigation Editor, notice that andorid studio has added this destination to the graph. 

