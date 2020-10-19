# Create Destinations
You can create a destination from an existing fragment or activity. You can also use the Navigation editor to create a new destination or create a placeholder to later replace with a fragment or activity. 

## Create a destinatio from an existing fragment or activity
In the Navigation Editor, if you have an existing destination type that you'd like to add to your navigation graph, click New Destination and then click on teh corresponding destination inthe dropdown that appears. You can now see a preview of the destination in the Design view along with the corresponding XML in the Text View of your navvigation graph. 

## Create a new fragment destination
To add a new destinatio type using the Navigation editor, do the following
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
You can use placeholders to rperesent unimplemented destinations. A placeholder serves as a visula representation of a destination. With in the Navigation editor, you can use placeholders just as you would any other destination. 

