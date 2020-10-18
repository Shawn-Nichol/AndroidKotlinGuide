# Local Persistence

Use local persistence to handle process death for complex or large data.

Persistent local storage, such as a database or shared preferences, will survivie for as long as your application is installed on the user's device (unless the user clears the data for your app). While such local storage survives system-initiated activity and application process death, it can be expensive to retrieve becuase it will have to be read from local storage in to memory. Often this persistence local storage may already be a part of your application architecture to store all data you don't want to lose if you open and close the activity. 

Neither ViewModel nor saved instance state are long-term storage solution and thus are not replacements for local storage, such as a database. Instead you should use thsee mechansisms for temporarly storing transisent UI state only and use persistent sotrage for other app data. 
