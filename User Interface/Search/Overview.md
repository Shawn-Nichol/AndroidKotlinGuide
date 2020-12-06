# Search Overrview
The search framework offers two modes of search input: a search dialog at the top of the screen or a search widget that you can embed in your activity layout. In either case, the Android system will assist your search implementation by delivering search queries to a specific activity that performs searches. you can also enable either the search dialog or widget to provide search suggestions as the  user types. 

Once you've set up eithe rthe search dialog or the search widget, you can 
- enable voice search
- Provide search suggestions based on recent user queries. 
- Provide custom search suggestions that match actauly resuults in your application data. 
- Offere you application's search sugggestion in the system-wide Quick search box. 

Note: THe search famework does not procide APIs to search your data. TO peform a search, you need to use APIs appropritate for your Data. For example, if you data is stored in an SQLite database, you should use the android.database.sqlite

Also there is no quarantee that a device provides a dedicated serach button that invokes the search interface in your applicaiton. When  using the serach dialog or a custom interface, you must provide  asearch button in your UI that activates the serach interface.

