# Create a Search Interface
When you're ready to add search functionality to your aplication, Android helps you implement the user interface with either a search dialog that appears at the top of the activity windows or a serarch widget that you can insert in your layout. Both the search dialog and the widget can deliver the user's search query to a specific activity inyour application. This way, the user can initiate a search from any activity where the search dialog or widget is avaiablable, and the system sstarts the appropriate activity to perfrom the search and prseent results. 

## Basics
Search dialog is a UI componbet that's controlled by the Anroid system. When activated by the user, the search diloag appears at the top of the activity. 

The android system controls  all events in the search dialog. When the user submits a query, the system delivers the query to athe activity that you specify to handle searches. The dialog can also provide search suggestions while the user types. 

- The search widget is an instance of SearchView that you can place anywhere in your layout. By default, the search widget behaves like a standar EditText Widget and doesn't do anything, but you can configure it so that the Android system handles all input events, delivers queries to the appropriate activity, and provides earch suggestions. 

When the user executes a search from the search dialog or a search widget, the system createsan INtent and stores the user query in it. THe ssytem then starts the activity that you've declared to handle searches and delivers it the intent. To set  up your application from this kind of assited search oyu need thte following. 

- A searchable configuration
An XML file that confiures some setting sfromt he search dialog or widget. It includes settings for features such as voice, suggestions, and hint text for the search box. 
- A searchable activity
The activity that receives the search query , searches your data and displays the search results
- A search interface, 
  - The search dialog
  By default, the search dialog is hidden, but apperas at the top of the screen when you call onSearchREquesteed()
  - Or a Search view Widget 
  Using the search widget allows you to put the search box anywhere in your activity. Instead of putting it in your activity layout, you should usually use search View as an action view in teh app bar. 
  
 ## Creating a searchable configuration 
