# Navigation Graph 
Navigation Graph shows a visual representation of the apps destinations and actions. Each destination is represented by a preview thumbnail and connection actions are represeented by an arrows that show how users can navigate from one destination to another. 

`Destination:` Are different contents areas in your app.

`Actions:` are logical connections between your destinations that represent paths that users can take. 


## Create a Navigation graph
1. Right-click res directory, select New > Android Resource File. The New Resource File dailog appears.

2. Give the file a name, such as nav_graph

## Navigation Editor. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            android:id="@+id/nav_graph">

</navigation>
```
Here you'll see corrsponding destinations and actions elements. 


# Nested Graphs
Sub-flows witin your app are usually best represented as nested navigation graphs. By nesting self-contained sub-navigatino flows in this way, 

If you have screens that only need to be viewed the first time the app is launched it is best practice to set those screens into a nested graph. 

Another way to modularize the graph structure is to include one graph within another via <inclue> element in the parent navigation graph. This allows the included graph to be defined in a separate module or project altogether, which maximizes reusability. 









