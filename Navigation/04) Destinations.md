# Destinations
You can create a destination from an existing fragment or activity. You can also use the Navigation editor to create a new destination or create a placeholder to later replace with a fragment or activity. 


## Create a destination
If you would like to add a destination to the NavGraph, click New Destination and then click on the corresponding destination in the dropdown that appears.

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
- Type: indicaates whether the destnation is implemented as a fragment, activity or other custom class in your soucred code. 
- ID: contains the ID of the destination which is used to refer to the destinatino in code. 
- Name: is location of the folder location of the destination.
- Label: contains the name of the destination's XML layout file

## Start Destination
The start destination is the first screen users see when opening your app, and its the last screen users see when exiting your app. The navigation editor uses a house icon to indicate the start destination. 


Select Start destination.
1. Desing tab, click on the destination to hightlight it
2. Click the assign start destinatino button, Alternatively, you can right-click on the destination and click Set as STart Destination. 


## PlaceHoler destinations
You can use placeholders to reperesent unimplemented destinations. A placeholder serves as a visual representation of a destination. Within the NavigationEditor, you can use placeholders just as you would any other destination. 

## Dialog Destination
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





