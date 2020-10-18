## Restoring complex states: reassembling the pieces

When it is time for the user to return to the activity, there are two possible scenarios for recreating the activity. 

- The activity is recreated after having been stopped by the system. The activity has the query saved in a onSaveInstanceState bundel, and should pass the query to the ViewModel. The ViewModel sees that it has no search results cached, and delegates loading the search results, using the given search query. 

The activity is created after a configuration change. The activity has the query saved in an onSavedInstnaceState Bundel, and the ViewModel already has the serarch results cahced. You pass the query from the onSaveInstanceState()) bundle to the ViewModel, which determines that it has loaded the necessary data and that it does not need to requery the database. 


