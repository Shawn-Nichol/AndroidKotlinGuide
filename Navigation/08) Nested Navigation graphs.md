# Nested Navigation graphs
A series of destinations can be grouped into a nested graph within a parent navigation graph called the root graph. Nested graphs are useful to organize and re-use sections of your app's UI, such as a self-contained loing flow. 

The nested graph encapsulates its destinations. As with a root graph, a nested graph must have a destiantion identified as the start destinations. Destinations outside of the nested graph, such as those on the root graph, access the nested graph only through its start destination 

To group destinations into a nested graph, do the following. 
1. In the Navigation editor, press and hold the shift key, and click on the destinations you want to include in the nested graph. 

2. Right-click to open the context menu, and select Move to Nested Graph > new Graph. The destinations are enclosed in a nested graph

3. Click the nested graph. The following attributes apear in the attributes panel
- Type, which contains Nested graph
- ID which contains a system-assigned ID for the nested graph. This ID is used to reference the nested graph from your code. 

4. Double-click on the nested graph to show its destinatinos. 
5. Click the text tab to toggle to the XML view. A nested navigation graph has been added to the graph. This navigation graph has its own navigation elements, along with its ownID and a startDestiation attribute that points to the first destinatino in the nested graph. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    app:startDestination="@id/mainFragment">
    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.cashdog.cashdog.MainFragment"
        android:label="fragment_main"
        tools:layout="@layout/fragment_main" >
        <action
            android:id="@+id/action_mainFragment_to_sendMoneyGraph"
            app:destination="@id/sendMoneyGraph" />
        <action
            android:id="@+id/action_mainFragment_to_viewBalanceFragment"
            app:destination="@id/viewBalanceFragment" />
    </fragment>
    <fragment
        android:id="@+id/viewBalanceFragment"
        android:name="com.example.cashdog.cashdog.ViewBalanceFragment"
        android:label="fragment_view_balance"
        tools:layout="@layout/fragment_view_balance" />
    <navigation android:id="@+id/sendMoneyGraph" app:startDestination="@id/chooseRecipient">
        <fragment
            android:id="@+id/chooseRecipient"
            android:name="com.example.cashdog.cashdog.ChooseRecipient"
            android:label="fragment_choose_recipient"
            tools:layout="@layout/fragment_choose_recipient">
            <action
                android:id="@+id/action_chooseRecipient_to_chooseAmountFragment"
                app:destination="@id/chooseAmountFragment" />
        </fragment>
        <fragment
            android:id="@+id/chooseAmountFragment"
            android:name="com.example.cashdog.cashdog.ChooseAmountFragment"
            android:label="fragment_choose_amount"
            tools:layout="@layout/fragment_choose_amount" />
    </navigation>
</navigation>

```

6. In your code, pass the resource ID of the action connecting the root grpah to the nested graph
```
view.findNavcontroller().navigate(R.id.action_mainfRagment_to_sendMoneyGraph)
```

7. Back in the Design tab, you can return to the root graph by clicking Root. 

## Reference other navigation graphs with <include>
Within a navigation graph, you can reference other graphs by using include. While this is functionally the same as using a nested graph, include lets you use graphs from other project modules or from library progjects. 

```
<!-- (root) nav_graph.xml -->
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/fragment">

    <include app:graph="@navigation/included_graph" />

    <fragment
        android:id="@+id/fragment"
        android:name="com.example.myapplication.BlankFragment"
        android:label="Fragment in Root Graph"
        tools:layout="@layout/fragment_blank">
        <action
            android:id="@+id/action_fragment_to_second_graph"
            app:destination="@id/second_graph" />
    </fragment>

    ...
</navigation>
```
```
<!-- included_graph.xml -->
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/second_graph"
    app:startDestination="@id/includedStart">

    <fragment
        android:id="@+id/includedStart"
        android:name="com.example.myapplication.IncludedStart"
        android:label="fragment_included_start"
        tools:layout="@layout/fragment_included_start" />
</navigation>
```
