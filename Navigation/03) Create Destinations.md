# Create Destinations
You can create a destination from an existing fragment or activity. You can also use the Navigation editor to create a new destination or create a placeholder to later replace with a fragment or activity. 

 
## Create a new fragment destination
In the Navigation Editor, if you have an existing destination type that you'd like to add to your navigation graph, click New Destination and then click on the corresponding destination in the dropdown that appears.

1) In the Navigation Editor, click the New destination icon and then click create new destination.

2) IN the New Android Component dialog that appears, createe your fragment.


## Create a destination From a DialogFragment
If you have an existing DialogFragment, you can use the dialog element to add the dialog to your navigation graph. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            android:id="@+id/nav_graph">

...
<dialog
    android:id="@+id/my_dialog_fragment"
    android:name="androidx.navigation.myapp.MyDialogFragment">
    <argument android:name="myarg" android:defaultValue="@null" />
        <action
            android:id="@+id/myaction"
            app:destination="@+id/another_destination"/>
</dialog>

...

</navigation>
```

## PlaceHoler destinations
You can use placeholders to reperesent unimplemented destinations. A placeholder serves as a visula representation of a destination. With in the Navigation editor, you can use placeholders just as you would any other destination. 

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

## Start Destination
The start destination is the first screen users see when opening your app, and its the last screen users see when exiting your app. The navigation editor uses a house icon to indicate the start destination. 

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
