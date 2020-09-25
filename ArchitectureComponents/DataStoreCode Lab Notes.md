DataStore is new and imporved data storage solution aimed at replacing SharedPreferences. Build on Kotlin coroutines and Flow, DataStore provides two differen implementations: Proto DataStore, that stores typed objects (backed by protocol buffers) and preferences DataStore, that stores key-valuepairs. Data is stored asynchronously, consistently, and transactionally, overcoming some of the drawbacks of SharedPreferences. 


DataStore is new and imporved data storage solution aimed at replacing SharedPreferences. Build on Kotlin coroutines and Flow, DataStore provides two differen implementations: Proto DataStore, that stores typed objects (backed by protocol buffers) and preferences DataStore, that stores key-valuepairs. Data is stored asynchronously, consistently, and transactionally, overcoming some of the drawbacks of SharedPreferences. 

Preferences vs Proto DataStore
While both preferences and proto datastore allow saving data, they do this in different ways. 
- Preference DataStore, like SharedPreferences, accesses data based on keys, without defining a schema upfonrt
- Proto DataStore defines the schema using Protocol buffers. Using Protobufs allows persisting stonrgly typed data. They are faster, smaller, simpler, and less ambiguous than xml and other similar data formats. While Proto brought by Proto DataStore is worth it. 

Room vs DataStore
If you ahve a need for partial updates, referential integrity or large/complex datasets, you should consider using Roomo in stead of DataStore. DataStore is ideal for small or simple datasets  and does not support partiial updates or refereential integrity. 

Preferences DataStore API is similar to SharedPreferences with several notable differences
- Hanles data updates transactionally
- Exposes a Flow respresenting the current state of data
- Does not have data persistenet methods (apply(), commit())
- Does not return mutalbe references to its internal state
- Expsoses an API similar to Map and MutableMap with typed keys.


Adding Dependencies
implementation "androidx.datastore:datastore-preferences:1.0.0-alpha01"

Althought both the show completed and sort order flags are user preferences, currently they're represented as two differenct objectts. So one of our goals is to unify these two flags under a UserPReferences class and store it in UserPReferenceRepository using DataStore. Right now the show  completed flag is kept in memory, in TasksViewModel.

Lets start by creating a UserPReferfences data class in UserPReferences Reposiotry. For now it should have one filed showCompleted. We'll add the sort order later

## Create the data store
Lets create a DataStore<Preferences> Private field in UserPReferencesReposiotyr, using the context.createDataStoreFactory() method. The mandatory parameter is the anme of the preferences DataStore.
  
  ## Reading Data from the Preferences DataStore
  Preferences DataStore expsoses the data stored in a Flow<Preferences> that will emit every time a preferences has changed. We don't want to expose the entire Preferences object but rather the UserPReferences object. To do this, we'll have to map the Flow<Preferences>, get the Boolean value we're interested in, basedd on a key and construct a UserPReference object. 
  
So, the first thing we need todo is define the show completed key - this is a pReferences.key<Boolean> value that we can declare as a member in a private PreferencesKey object


Lets expose a UserPReferenceFlow: Flow<USerPReferences>, constructed based on dataStore.data:Flow<Preferences>, which is then mapped to retrieve the right preference:
  
## Handling exception while reading data
As DataStore reads data from a file, IOExceptions are thrown when an error occurs while reading data. We can handle these by using the  catch(). Flow operator before map() and emitting emptyPreferences() in case the exception thrown was an IOException. If a different type of exceptioin was throw, prefer re-throwing it. 

## Writing data to Preferences DataStore
To write data, DataStore offers a suspending DataStore.edit(Transform: Suspend (MutablePreferncees -> Unit) function, which accepts a transform block that allows us to transactionally update the state in DataStore. 

The MutablePReferences passed to the transform block will be up-todate with any previously run edits. All changes to MutablePreferences in teh transform block willl be applied to disk after transform complete and before edit completes. Setting one value in MutablePRefernces will leave all other preferences unchanged

Note: Do not attempt to modify the MutablePrefernces outside of the transform block. 

Lets cretae a suspend function that allows us to update the show Completed property of USerpREferences, calle dupdateShowCompleted(), that calls dataStore.edit9) and sets the new value

Edit9) can thow an IOExceptioin if an error was encoutnered whilie reading or writing to disk. If any other error happens tin the tranform block it will be thwon by edit()
At this point, the app should compile but the functionality we just created in UserPRefernces REpository is not used. 
