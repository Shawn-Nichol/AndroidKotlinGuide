## Action
An action is a logical connection between destinations. Actions are represented in the navigation graph as arrows. Actions usually connect one destination to another, you can aslo create global action that take you to a specific destination from anywhere in your app. Action represent different paths that users can take though your app. Note to navigate to another destination you still need to write the code to perform the navigation.


## Connect destinations
You can use the Navigation editor to connect two destinatioons.

1. Click and drag your cursor over the destination you want users to navigate to, and release. The resulting line between the two destinations represents an action. 

2. Click on the arrow to highlight the action. The following attributes appear in the Attributes pane, at minium, an action contains its own ID and the ID of the destination to which a user should be taken.
- Type: Action
- ID: the ID for the action.
- Destination: The ID for the destination fragment or activity. 

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



## GLobal actions
Any destination in your app that can be reached by mutliple paths should have a global action defined to navigate to that destination. Global actions can be used to navigate to a destination from anywhere. 

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   android:id="@+id/in_game_nav_graph"
   app:startDestination="@id/in_game">

   <!-- Action back to destination which launched into this in_game_nav_graph-->
   <action android:id="@+id/action_pop_out_of_game"
                       app:popUpTo="@id/in_game_nav_graph"
                       app:popUpToInclusive="true" />

   <fragment
        ...
   </fragment>


</navigation>
```
