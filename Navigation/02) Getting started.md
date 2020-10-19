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

## Anatomy of a destination
Click on a destination to select it and note ht following attribute in the attributres panel 
- The type field indicaates whether the destnation i simplemented as a fragment, activity or other custom class in your soucred code. 
- The label field contains the name of the destination's XML layout file
- The ID field contains the ID of the destinatino which is used to refer to thee destinatino in code. 
- The class dropdown shows the name of the class  that is associated with thedestinationn. You can click this dropdown to change the associated class to another ddestination type. 

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    app:startDestination="@id/blankFragment">
    <fragment
        android:id="@+id/blankFragment"
        android:name="com.example.cashdog.cashdog.BlankFragment"
        android:label="Blank"
        tools:layout="@layout/fragment_blank" />
</navigation>
```

## Designate a screen as the start destination
The start destination i sthe first screen users see hwen opening your app, and its the last screnn users wee ehen exiting  your app. The navigation editor uses a house icon to indicate the start destination. 

Once you have all of your destination in place, you can choose a start destination by doing the following

1. Desing tab, click on the destination to hightlight it
2. Click the assign start destinatino button, Alternatively, you can right-click on the destination and click Set as STart Destination. 

## Connect destinations
An action is a logical connection between destinations. Actions are represented in the navigation graph as arrows. Actions usually connect on e destination to another, though you can also create global actions that take you to a specific destination from aywhere in your app. 

With actions, you're representing the different paths that users can take through you can also create global actions that take you to a specific destination from anywehre in your app. 

With actions, you're representing the different paths that users can take through your app. Note that to actually  connect one destination to another,though you can also create global actions that take you to a specific destination from anywhere in your app. 

With actions, you're representing the differen tpaths that users can take through your app. Note that to acutally navigate to destinations, you still need to wwrite the code to perform the navigation. This is covered in the Navigate to a destination section. 

You can use the Navigation editor to connect two destinatioons by doing the following. 

1. in the desgine tab, hover over the right side of the destination that you want users to navigate from. A circle appears over the right side of the destination, 

2. Click and drag your cursor over the destination you want users to navigate to, and release. The resulting line between the two destinations represents an action. 

3. Click on the arrow to highlight the action. The following attributes appear in the Attributes panel
- the type filed contains "Action"
- The ID field contains the ID for the action.
- Th edestination field contains the ID for the destination fragment or activity. 

4. Click the Text tab to toggle to the XML view. An action element is now added to the source destination. The action has an ID and a destination attrbitue that contains the ID of the next destination, as shown in the example below. 

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    app:startDestination="@id/blankFragment">
    <fragment
        android:id="@+id/blankFragment"
        android:name="com.example.cashdog.cashdog.BlankFragment"
        android:label="fragment_blank"
        tools:layout="@layout/fragment_blank" >
        <action
            android:id="@+id/action_blankFragment_to_blankFragment2"
            app:destination="@id/blankFragment2" />
    </fragment>
    <fragment
        android:id="@+id/blankFragment2"
        android:name="com.example.cashdog.cashdog.BlankFragment2"
        android:label="fragment_blank_fragment2"
        tools:layout="@layout/fragment_blank_fragment2" />
</navigation>
```
In your navigation graph, actions are represented by <action> elements. At a minium, an action contains its own ID and the ID of the destination to which a user should be taken. 
  
## Navigation to a destination
Navigation to a destination is done using a NavController, an object hat manages app navigation within a NavHost. Each NavHost has its own correspondingn Navcontroller. you can retireve a NavController by uing one of the following methos

Kotlin
- Fragment.findNavController()
- View.findNavController()
- activity.findNavController()

When createing the NavHostFragment using FragmentContainerView or if manually adding the NavHostFramgent to your activity via a FragmentTransaction, attempting to retrieve the NavController in onCreate(0 of an activity via navigation.findNavController(Activity, #EdRes int) will fail. You should retrieve the NavController directly from the NavHostFragment instead. 

```
val navHostFragment = suppoortFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
val navController = navHostFragmennt.navController. 
```
## Ensure type save args
The recommend way to navigate beteween destinations is to use the Safe Args Gradle plugin. This pluging generates simple object and builder classes that enable type-safe navigation and argument passing between destinations. 

To add SafeArgs to your progject, includ the following classpath in your top level build.gradle file. 
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

You must alos apply one of two available plugins

To generate java language cod sutiable for Java or mixed Java and Kotlin modules, add this line ot your app or module's build.gradle. 
```
apply plugin: "androidx.navigation.safeargs"
```

Alternatively, to generat kotlin code suitable for kotlin-only modules add
```
apply plugin: "androidx.navigation.safeargs.kotlin"
```

You must have android.useAndroidX=true in your gradle.properties file as per Migrating to AndroidX

After you enable SafeArgs, the plugin generats code that contains classes and methods for each action youv'e defined. For each actions, safe args also generates a class for each originating destnation, which is the destniation from twhich the action orignates. The generated class names is a combination of originating destination class name and the word "Directions'> For example, if the destination i snamed SpedifyAmountFragmnet, the generated class is naed SpecifyAmountFragmentDirections. The generated class contains a static method for each action define din the originating destination. The generated class contains a staic method for each action defined in the orignating destination. This method takes any defined action parameteres as arguments and returns a NavDirections object that you can pss to navigate(). 

As an example, assume we have a navigation graph with a single ation that connets th originating destination, SpecifyAmountFragment,  to a receiving destination, ConfirmationFragment. 

Safe Args generats a SpecifyAmountFragmentDirections class with a single method, actionsSepcifyAmountFragmentTo confirmationfRagment(0 that retusn a NavDirections object. This returned NavDirections object can bthe be passed directly to navigate(), as shown, 

```
override fun onClick(view: View) {
  val action = SpecifyamountFragmentDirections.actionSepcifyAmountFragmentToConfirmationFragment()
  view.findNavController().navigate(action)
}

```

For more information on passing data between destination with safe Args 
