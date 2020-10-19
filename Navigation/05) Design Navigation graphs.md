# Deisgn navigation graphs 
The Navigation component uses a navigation graph to manage your app's navigation. A navigation grpah is a resource file that contains all o f your app's destinations along with the lgoical connections, or actions, that users can take to navigte from one destination to another. You can manage your app's navigation graph using the Navigation Editor in Android stuido. 

## Top-level navigation graph
Your app's top-level navigation graph should start with the initial destination the user sees when launching the app and should include the destinations that they see as they move about your app. 


# Nested Graphs
Login flows, wizards, or other sub-flows witin your app are usually best represented as nested navigation graphs. By nesting self-contained sub-navigatino flows in this way, the main flow of your app's UI is easier to comprehend and manage. In addition, nested graphs are reusable. They also provide a level of encapsulation -destinations outside of the nested graph do not have direct access to any of the destinations iwthin the nested graph. insteaed, they should navigate() to the nested graph itself, where the internal logic can change without affecting the rest of the graph. 

Using the top-level navigation graph, if you have screens that only need to be views the first time the app is launched it is best practice to set those screens into a nested graph. 

Another way to modularize your graph structure is to include one graph within another via <inclue> element in the parent navigation graph. This allows the included graph to be defined in a separate module or progject altogether, which maximizes reusability. 

## navigation across library modules. 
If your app depends on library modules, which have a nvigation graph contained therein, you can reference these navigation graphs using an <include element. 

Buiding on our triva game example, let's say you want to separate the game-centrix parts of the app into a separate library module so that you can include the in_game, results_winner, and game_over screens in multiple apps, as show

```
<!-- App Module Navigation Graph -->
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
           xmlns:app="http://schemas.android.com/apk/res-auto"
           xmlns:tools="http://schemas.android.com/tools"
           app:startDestination="@id/match">

   <fragment android:id="@+id/match"
           android:name="com.example.android.navigationsample.Match"
           android:label="fragment_match">

       <!-- Launch into In Game Modules Navigation Graph -->
       <action android:id="@+id/action_match_to_in_game_nav_graph"
           app:destination="@id/in_game_nav_graph" />
   </fragment>

   <include app:graph="@navigation/in_game_navigation" />

</navigation>
```

```
<!-- Game Module Navigation Graph -->
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   android:id="@+id/in_game_nav_graph"
   app:startDestination="@id/in_game">

   <fragment
       android:id="@+id/in_game"
       android:name="com.example.android.gamemodule.InGame"
       android:label="Game">
       <action
           android:id="@+id/action_in_game_to_resultsWinner"
           app:destination="@id/results_winner"  />
       <action
           android:id="@+id/action_in_game_to_gameOver"
           app:destination="@id/game_over"  />
   </fragment>

   <fragment
       android:id="@+id/results_winner"
       android:name="com.example.android.gamemodule.ResultsWinner" >

       <!-- Action back to destination which launched into this in_game_nav_graph-->
       <action android:id="@+id/action_pop_out_of_game"
                           app:popUpTo="@id/in_game_nav_graph"
                           app:popUpToInclusive="true" />

   </fragment>


   <fragment
       android:id="@+id/game_over"
       android:name="com.example.android.gamemodule.GameOver"
       android:label="fragment_game_over"
       tools:layout="@layout/fragment_game_over" >

      <!-- Action back to destination which launched into this in_game_nav_graph-->
      <action android:id="@+id/action_pop_out_of_game"
                          app:popUpTo="@id/in_game_nav_graph"
                          app:popUpToInclusive="true" />


 </fragment>

</navigation>
```

## GLobal actions
Any destination in your app that cna be reached through more than one path should have a corresponding global action defined to navigate to that destination. Global actions can be used to navigate to a destination from anywhere. 

Let's apply this to our library module sample, which has the same actions defined in both ethe win and game over destinatinos. you should extract these common actions to a single global action and reference them from both destiations.

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
       android:id="@+id/in_game"
       android:name="com.example.android.gamemodule.InGame"
       android:label="Game">
       <action
           android:id="@+id/action_in_game_to_resultsWinner"
           app:destination="@id/results_winner" />
       <action
           android:id="@+id/action_in_game_to_gameOver"
           app:destination="@id/game_over" />
   </fragment>

   <fragment
       android:id="@+id/results_winner"
       android:name="com.example.android.gamemodule.ResultsWinner" />

   <fragment
       android:id="@+id/game_over"
       android:name="com.example.android.gamemodule.GameOver"
       android:label="fragment_game_over"
       tools:layout="@layout/fragment_game_over" />

</navigation>
```
