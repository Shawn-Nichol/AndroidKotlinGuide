# Content Providers
Content providers can help application maange access to data stored by itself, stored by other apps, and provide a way to share data with other apps. They encapsulate the data, and provide mechanisms for defining data security. Content Providers are the standard interface taht connects data in one process with code running in another process. Implementing a content provider has many advantages. most importanly you can conifgure a content provider to allow other application to securly access and modify your app data. 

Use content providers if you plan to share data. If you don't plan to share data, you may still  use them becuase they provide a nice abstraction, but you odn't have to. This abstraction allows you to make modifications to your application data storage implementation without affecting other existing applictions that rely on acccess to your data. in this scenario only your content provider is affected and not the applications that access it. For example, you might swap out a SQLigte database for alternative storage. 

A Number of other classes relpy on the ContentProvider class. 
  - AbstractThreadSyncAdapter
  - CursorAdapter
  - CursorLoader

If you are making use of any of these classes you also need to implement a content provider in your application. Note that when working with the sync adapter framework you can also create a stub content provider as an alternative. For more information about this topic see creating a stub content provider. In addition, you need your own content proivder in the following cases
- you want to implemetn custom search suggestions in your application
- you need to use a content provider to expose your application data to widgets
- You want to copy and paste complex data or files from your application to other applications

The Android fremwork includes content providers that manage data such as audio, video, images and personal contact information. You can see some of them listed in the reference documentation for the android.provider package. With some restrictions, these providers are accessible to any Anroid application. 

A content provider can be used to manage access to a variety of data storage sources, including both structured data, such as a SQLite relational database, or unstructured data such as image files. For more information on the types of storage availlable on Android, see Storage options as well as Designing data storage. 

## Advanatages of content providers
Content providers offer granular control over the permissions of accessing data. You can choose to access data from other aplications, or conifure different permissions for reading and writing data. For more information on using content providers securely, see Security tips for storing data, as well as Content provider permissions. 

You can use a content provider to abastract away the details for accesssing different data sources in your application. For example, your application might store structured records in a SQLite database, as well as video and audio files. You can use a content provider to access all of this data if you impelemtn this development pattern in your application also note that CursorLoader objects rely on content provider to run asynchronous queries and then return the results to the UI llayer in your application. For more information on using a CursorLoader to load data in the background, see Running a query with a CursorLoader. 



# Content Provider basics
A content provider mamanages access to a central repository of data.  A provider is part of an Androidi application, which often provides its own UI for working with the data. However, content providerrs are primariliy intended  to be used by other applications, which access the provider using a provider client object. Together, providers and provider clients offer a consistent, standeard interface to data that also handles inter-process communication and secure data access. 

Typically you work with content providers in one of two scenarios; you may want to implement code to access an existing content provider in another application, or you may want to create a new contnet provider in your application to share data with other applications. THis topic covers the basisc s of workign with existing content providers. To learn more about implementing content providers in your aown applications, see Creating a content provider. 

## Overview
A content provider presents data to external applications as one or more tables taht are similar to the tables found in a relational database. A row represents an instance of some type of data the provider collects, and each column in the row represents an individual piece of data collected for an instance. 

A content provider coordinates accesss to the data storage layer in your application for a number of different APIs and components
- Sharing access to your application data with other applications
- Sendingn data to a widget
- Returning custom search suggestions for your application through the search framework using SearchRecentSuggestionsProvider
- Synchronizing application data with your server using an implementation of AbstractThreadedSyncAdapter
- Loading data in your UI using a Curosrloader

## Accessing a provider
When you want to access data in a content provider, you use the ContentResolver object in your applications Context to communicate with the provider as a client. The ContentResolver object communicates with the provider object, an instance of a class that implements COntentPRovider. The provider object reeives data requests from clients, performs the request actions, and retunrs the result. This object has methods that call identically-named methods in the provider object, an instance of one of the conrete subclasses of ContentProvider. The contentResolver methods provide the basic "CRUD" (create, retrieve, update, and delete) functiosn of persistent storage. 

Acommon patter for accessing a COntentProvider from your UI uses a CUrsorLoader to run an asynchronous query in the background. The activity or fargment in your UI call CUrsorLoader to the query, which in turn gets the ContentPRovider using the COntentResolver. This allows the UI to conitnue to be available to the user while the query is running. THis pattern involves the interaction of a number of different objects, as well as the underyling sotrage mechanism. 

Activity/Fragment <-> CursorLoader <-> ContentResolver <-> ContentProvider <-> DataStorage

Note: To access a provider, your application usually has to request specific permissions in the manifest file. This development pattern is described in more detail in the section Content Provider Permissions. 

One of the built-in providerse in the Android platform is the user ditionary, which stores the spelling of non-standard words that the users wants to keep.

Each ro represent an instance of a work that might not be found in a standard dictionary. Each colun represents some data for that word, such as the locale in which it was first encountered. The column headers are column names that are stored in the provider .To refer to a row's locale, you refere to its locale column. For this provider, the _ID column servies as a primary  key column that provider automatically maintans

To geta a list of the words and their locales fromt he User Dictionary Provider, you call ContentResolver.query(). The query() method calls the ContentProvider.query() methods defined by the User Dictiionary Provider. The following lines of code show a ContentResolver.query() call
```
// Queries the user ditionary adn retuns results
cursor = contentResolver.query(
  UserDictioinary.WOrds.CONTENT_URI, // Tehe content URI of the words table
  projection,  // The columns to return for each row
  selectionClause, // Selection criteria
  selectionArgs.toTypedArray(), // Selection criteria
  sortOrder // The sort order for the returned rows. 
  
```


## Content URIs
A content URI is a URI that identifies data in a provider. Content URIs include the symbolic name of the entire provider (its authority) and a name that points to a table(a path). When you call a client method to access a table in a provider, the content URI for the table is one of the arumgents. 

In the preceding lines of code, the constant CONTENT_URI contains the content URI of the user dictionary's "words" table. The ContentResolver object parses out the URI's authority, and uses it to "resolve" the provider by comparing the authority to a system table of knwn providers. THe ContentREsolver can then dispatch the query arguments to the correct provider. 

The ContentProvider uses the path part of the content URI to choose the table to access a provider usually has a path for each table it exposes. 

In the previos lines of code, the full URI ofr the "words" tables is
```
content://user_dictionary/words
```

where the user-ditionary string is the provider's authority, and the words string is the table's path. The string content:// (the scheme ) is alwasy present, and identifies this as a content URI. 

Many providers allow you to access a signle row in a table by appending an ID value to the end of the URI. Fore example, to retrieve a row whose _ID is 4 from user dicitioanry, you can use this contenet URI:

```
val singleUri: Uri = ContentUris. withAppendedId(UserDitioinary.WOrds.CONTENT_URI, 4)
```

You often use id values when you've retrieved a set of rows and then want to update or delete one of them

Note: The Uri and Uri.Builder classes contain convenience methods for constructing well-formed URI objects form strings. The ContentUris class contains convenience methods for appending id values to a URI. The previous snippet uses iwhtAppendedID() to append an id tothe User Ditionary content URI. 


## Retrieveing data form the provider
This section describes how to retrieve data from a provider, using the User Dictionary Proider as an example.

To retrieve data from a provider, follow these basic steps.
1) request the read access permission for the provider
2) Define the code that sends a query to the provider

### Requesting read access permission
To retrieve data from a provider, your application needs "read access permission" for the provider. You can't request this permission at run-time; instead, you have to specify that you need this permission in your manifest, using the <uses-permission> element and the exact permission name defined by the provider. When you specify this element in your manifest, you are in effect "requesting" this permission for your application. When users install your application, they implicitly grant this request
  
To find the exact name of the read access permission for th provider you're using, as well as the names for other access permissions used by the provider, lookk in the providers documentatioin.
  
The role of permissions in accessing providers is described in more detail in the section Content provider permissions
  
The User Dictionary Provider defines the permissino and.permission.READ_USER_DICTIONARY in its manifest file, aos an application that wants to read from the provider must request this permission.
  
### Constructin the query
The next step in retrieving data from a provider is to construct a query. This first snippet deines some variables for accessing the User Dicitonary Provider. 
```
// A "progjection" defines the columns that will be returned for each ro
private val mProjection: Array<String> = arrayOf(
  UserDictionary.Words._ID // Contact class constant for the _ID column name
  UserDicitionary.Words.WORD, // Contact class constant for the word column name
  UserDicitonary.Words.LOCALE // Contract class constant for the locale column name
)

// Defines a string to constain the section clause
private var selectionClause: String? = null

// Declares an array to contain selection arguments
private lateinit var selectionArgs: Array<String>
```

The next snippt shows how to use Content Resolver.query(), using the User Dictionary Provider as an example. A provider client query is similar to an SQL query, and it contains a set of columns to return, a set of selections criteria, and a sort order. 

The set of columns that the query should return is called a projection(the variable mProjection).

The expression that specifies the rows to retrieve is split into a selcection clause and selection arguments. The selection clause is combination of locial and Boolean expressions, column names, and values (the variable mSelectionClause). If you specify the replaeable parameter ? instead of a value, the query method retrieves the value from the selection arguments array (the variable mSelectionArgs).

In the next snippet, if the user doesn't enter a word, the selection clase is set to null, and the query returns all the words in the provider. If the users enters a word, the selection clause is set to UserDictionary.Words.WORD + " = ?"
and the first element of selection argumetns array is set to the word the user enters. 
```
/*
 *  This delcares string array to contain the selection arguments.
 */
 private lateinit var selectionArgs: Array<String>
 
 // Gets a word from the UI
 searchString = searchWord.text.toString()
 
 // Remember to insert code here to check for invalid or malicious input
 
 // IF the wor is the empty string, gets everything
 selectionArgs = searchString?.takeIf {it.isNotEmpty() }?.let {
  selectionClause = "${UserDictionary.Words.WORD} = ?"
  arrayOf(it)
 } ?: run {
  selectionClause = null
  emptyArray<String>()
  
  // Does a query against the table and returns a Cursor object
  mCursort = contentResolver.query {
    UserDictionary.Words.CONTENT_URI,  // The content URI of the words table
    projection,                       // The columns to return for each row
    selectionClause,                  // Either null, or the word the user entered
    selectionArgs,                    // Either empty, or the string the user entered
    sortOrder                         // The sort order for the returned rows
)

// some providers return null if an error occurs, others throw an exception 
when(mCursor?.count) {
  null -> {
    /*
     * Insert code here to hanlde the error. Be sure not use the cursor!
     * You may want to call android.util.Log.e() to log this error.
     */
  } 0 -> {
    /*
     * Insert code here to notify the user that the serach was unsuccessful. This ins't rnecessarily an error. 
     * You may want to offer the user the option to insert a new row, or re-type the search term.
     */
  } else {
    // Insert code here to do something with the results
    
  }
}
```

This query is analogous to the SQL staement
```
SELECT _ID, word, locale FROM words WHERE word = <userinput> ORDER BY  word ASC;
```

in this SQL statement, the actual column names are used instead of contract class constrants

### Protecting against malicious input
If the data managed by the content provider is an SQL database, including external untrusted data into raw SQL statemetns can lead to sQL injection
Consider this selection clause

```
// Constructs a selection clause by concatenating the user's input to the column name
var selectionClause = "var = $mUserInput"
```

If you do this, you're allowing the user to concatenate malicious SQl onto your SQL statement. Fore example, the user could enter "nothing: DROP TABLE;" for mUserInput, which would result in the selection clause var = nothing; DROP TABLE * ; ". Since the selectin clasue is treated a s an SQL stement, this might cause the provider to erase all of the tables in teh underlying SQLite database (unless the provider is et up to catch SQL injection attempts).

To avoid this problem, use a selection clause that uses? as a replaceable parametere and a separate array of selection arguments. When you do this the user input is bound directly to the query rather than being interpreted as part of an SQL statement. Becuase it's not eated as SQL, the user input can't inject malicious  SQL. Instead of using concatenation to include the user input, use this selection clause;

```
// constructs a selcetino cluase with a replacelable paramaeter
var selectionClause = "var = ?"
```

setup The array of selection arguemtns
```
// Defines a mutable list to contain the selectionarguments
var selectionArgs: MutabeList<String> = mutableListOf()
```

Put a value in the selection arguemtns array
```
selectionArgs += userInput
```

A selection clause that uses ? as a replaceable parametere and a array of selection argumetns arraay are preferred way to specify a selection, even if the provider isn't based on an SQLdatabase

### Dsiplaying query results
The contentResolver.quer() client method always returns a Cursor containing the columns specified by the query's projection for the rows that match the quer's selection criteria. ACursor object provides random read access to the rows and columns it contains. Using Cursor methods, you can iterate over the rows in the results, determine the data type of each column, get the data out of a column, and examine other properties of the results. Some Cursor implementations autotmatically update the object when the providers data changes, or trigger methods in an observer object when the Cursor changes, or both. 

Note: Provider may restrict access to columns based on the nature of the object making the query. Fore example, the Contacts Provider restricts access for some columns to sync adapters, so it won't return them to an activity or service.

If no rows match the selection criteria, the provider returns a Cursor object for which Cursor.getCount() is 0 ( an empty cursor)

If an internal error occurs, the results of the query depend on the particular provider. It may choose to return null, or it may throw an Exception. 

Since a Cursor is a list of rows a good way to display the contents of a Cursor is to link it ti a ListView via a SimpleCursorAdapter

The following snippet continues the code from the previous snippet. It creataes a SimpleCursorAdapter obejct containing the Cursor retrieved by the query, and sets this object to be the adapter for a ListView.

```
// Defines a list of columns to retrieve from the Cursor and load into an ouput ro
val wordListColumns : Array<String> = arrayOf(
  UserDictionary.Words.WORD, // Contactclass constant continaing the word column name
  UserDictionary.Words.LOCALE // Contract class constant continaing the locale column name
)

// Defines a list of View IDs will reeive the Cursor columns for eacch row
val wordListITems = intArrayOf(R.id.dictWord, R.id.locale)

// Creates a new Simple Cursor Adapter
cursorAdapter = SimpleCursorAdapter (
  applicationContet, // The applications Context boject
  R.layout.word.listrow, // A layout in XML for one row in the ListView
  mCursor, // The result from thw query
  wordListColumns, // A string array of columnnames in the cursor
  wordListITems, // An Integer array of view IDs in the row layout
  0, // Flags(usually non nare needed)
)

// Sets the adapter for the ListView
worList.setADapter(cursorAdapter)
```
Note: To back a ListView with a Cursor, the cursor must contain a column named ID  becuase of this, the query shown previously retrieves the ID column for the ListView doesn't display it. This restrictionalso explains why most prpviders have ID column for each of their tables. 

### Getting data from query results
Rather than simply displaying query results, you can use them for other tasks. For example, you can retrieve spellings from the user dictionary and then look them up in other providers. To do this, you iterate over the rows in teh Cursor:
```
/*
 * Only executes if the cursor is valid. The UserDictionary Profider returns null if an internal error occurs. Other providers may throw an Exception instead of returning null. 
 */
 mCursor?.apply {
  // Determine the column indeex of the column named "word"
  val index: Int = getColumnIndex(UserDictionary.Words.WORD)
  
  /*
   * Moves to the next row in the cursor. Before the first movement in the cursor, the "row pointer" is -1 and if you try to retrieve data at that poistion you 
   * will get an exception.
   while(moveToNext()) {
    // Gets the value form the column.
    newWord = getString(index)
    
    // Insert code here to process the retrieved word
    ...
    
    // end of while loop
    
   }
 }
```

Cusor implementations contain several "get" methods for rretrieving different types of data from the object. For example, the previous snippet uses getString(). They also have a getType() method that returns a vlaue indicating the data types of the column.

## Content provider permissions
A provider's application can specify permissions that other applications must have in order to access the providers data. These permissions ensure that the user knows what data an application willl try to access. Based on the provider's requiremnts, other applications request the permissions they need in order to access the provider. .End users see the requested permissions when they install the application. 

If a provider's application doesn't specify any permissions, then other application shave no access to the provider's dataa. However, components in teh provider's application always have full read and wirte access, regardless of the specified permissions. 

As noted previously, the User Dictionary PRovider requires the android.permission.READ_USER_DICTIONARY permission to retrieve data from it. The provider has the separate android.permission.WRITE_USER_DICTIONARY permissionf for inserting, updating or deleting data. 

To get the permissions needed to access a provider, an application requests them with a <uses-pemrission> element in tits manifest file. When the Android Package Manager installs the applications, a user must approve all of the permissions the application requests. If the user approves allof th tem, Package Manager continues the installation; if the user doesn't approve them, Package Manager aborts the installation
  
  The following <uses-permission> element requests read access o the User Dictionary Provider.
  ```
  uses-permission android:name="android.permission.READ_USER_DITIONARY">
  ```
  
  The impact of permissions on provider accessis explained in more detail in the Security and permissions guide. 
  
  ## Inserting, updating and deleting data
  
  In the same way that you retrieve data from a provider, you also use the interaction between a provider client and the provider's ContentProvider to modify data. You call a method of ContentResolver with arguments that are passed to the corresponding method of ContentProvider. The provider and proder client automatically handle security and inter-process communication. 
  
Inserting Data 
In the same way that you retrieve data from a provider, you also use the interaction betweeen a provider client and the provider's ContentProvider to modify data. You call a method of ContentResolver with arguments that are passed to the corresponding methods of ContentProvider. The provider and provider client automatically handle security and inter-process communication

### Inserting data
To insert datat in to a provider, you call the ContentResolver.insert() method. This method inserts a new row into the provider and returns a contentURI rof that row. This snippet shows how to insert a new word into the User Ditioinary Proivder. 

```
// Defines a new URI object that receives the result of the insertioin 
lateinit var newUri: Uri

...

// Defines an object to contain the new values oto insert
val newValues = ContentValues().apply {
  /*
   *Sets the values of each column and inserts the word. The arguments to the "put" 
   * method are column name" and value"
   */
   put(UserDictionary.Words.APP_id, "example.user")
   put(UserDictionary.Words.LCOAL, "en_US")
   put(UserDictionary.Words.WORD, "insert")
   put(UserDictionary.Words.FREQUENCY, "100")
}

new Uri = contentResolver.insert(
  UserDictionary.Words.CONTENT_URI, // The user dictionary content URI
  newValues // The values to insert
```
The data for the new row goes into a single ContentValues object, which is similar in form to a one-row cursor. The columns in this boject don't need to have the same data type, and if you don't want to specify a value at all, you can set a column to null using ContentValues.putNull().

The snippet doesn't add the ID column, becuase this column i s mainttained automatically. The provider assigns a unique value of ID to every row that is added. Providers usually use this value as the tables primary key. 

The content URI returnred in newUri identifies the newly-added row, with the following format
```
content://user-dictionary/words/<id_value>
```

The<id_value> is the contents of ID for the new row. Most providers can detect this form of content URI automatically and then permor the requeste operation on the particular row. 

To get the value of ID from the returned Uri, call ContentURis.parseID().

### Updating data
To updatea row, you can use a ContentValues object with the updated values just as you do with an insertion, and selection criteria just as you dow with a query. The client method you use is ContentResolver.update(). You only need to add values to the ContentValues object for columns you're updating. If you want to clear the contents of a column, set the value to null. 

The following snippet changes all the rows whose locale has the language"en" to a have a local of null. The return value is the number of rows that were updated

```
// Defines an object to contain the updated values
val updateValues = ContentValues().apply {
  /*
   *sets the updated value  and updates the selected words.
   */
   putNull(UserDictinoary.Words.LOCALE)
}

// Defines selection criteria for the rows you want to update
val selectionClause: String = UserDictionary.Words.LOCALE + "LIKE ?" 
val selectionArgs: Array<String> = arrayOf("en_%")

// Defines a variable to contain the number of updated rows
var rowsUPadted: Int = 0

...

rowsUpdated = contentResolver.update(
  UserDictionary.Words.CONTENT_URI, // The user ditionary content URI
  updateValues, // The columns to update
  selectionClause, // the column to select on
  selectArgs // the value to compare.
)
```

You shoould also sanitize user input when you call ContentResoler.update(). To learn more about this, read the section Protecting against malicious input. 

## Deleting data
Deleting rows is similar to retrieving row data: you specify selection criteria for the rows you want to delete and the client method returns the number of deleted rows. 

```
// // Defines selection criteria for the rows you want to delete
val selectionClause = "${UserDictionary.Words.lOCAL} LIKE ?"
val selectionArgs: Array<String> = arrayOf("user")

// Defines a variable to contain the number of rows delted
var rowsDeleted: Int = 0

...

// Deletes the words that match the selection criteria
rowsDeleted = contentResolver.delete(
  UserDitionary.Words.CONTNET_URI, // The user dictionary content URI
  selectionClause, // The column to select on
  selectionArgs // the value to compare to
)  
```
 You should also sanitize user input when ou call ContentResolver.delete().
 
 ## Proivder Data Types
 Content providers can offer many differen t data types. The User Dictionary Provider offers only text, but providders can also offer the following formats
 - String
 - Integer
 - long
 - floating piont
 - double
 
 Another data type that prviders often use is Binary LargeObject(BLOB) implemented as a 64KB bytes array. You can see the available data types by looking at the Cursor class "get"methods. 
 
 The data type for each column in a provider is usually listed in its documentation. The datat types for the User Dictionary Provider are listed in the referrence documentation. The data types for the User Dictionary PRovider are listed in the reference documentation for its contaract class UserDictionary.Words(contract classes are described in the section Contract Classes). You can also determine the data type by calling Cursor.getType()
 
 Providers alos maintain MIME data type information for each content URI they define. you can use the MIME type information to find out if your application can hadnle data that the provider offers, or to choose a type of handling based on the MIME type. You usually need the MIME type when you areworking with a provider that contains complex data structures or files. For example, the ContactsContract.Data table in the Contacts Provider uses MIME types to label the type of contact data stored in each row. To get the MIME type corresponding to a content URI, call ContentResolver.getType(). The section MIME type reference descibes the syntax of both standard and custom MIME types
 
 ## Alternative forms of providers access
 Three alternative forms of provider accesss are importanat in application development.
 - Batch access: You can create a batch of access calls with mehods in the ContentProviderOperation class, and then apply them with ContentResolver.applyBatch()
 - AsynChronous queries: you should do queries ina separate thread. One way todo this is to use a CursorLoader object. Theexamples in th eLoaders guid demonstrate how to do this. 
 - Data access via intents: Although you can't send an intent directly to a prvoider, you can send an intent to the provider's application, which is usually the best-equipped to modify the providers data. 
 
 Batch access and modification via intents are desribed in the following sections
 
 ### Batch access
 Batch access to a provider is useful for inserting a large number of rows, or for inserting rows in multiple tables in the same method call, or in general for performing a set of operations across process boundaries as a transcation(an atomicoperationo)
 
 To access a provider in "batch mode", you create an array of ContentPRoviderOperation objects and then dispatch them to a content Provider with ContentResolver. applyBatch(). You pass the content provider's authority to this method, rather than a particular content URI. THis allows each ContentProicderOperation object in the aray to work against a different table. A call to ContentResolver.applyBatch() returns an array of results
 
 The desctiption of the ContactsContract.RqwContacts contract class includes a code snippet that demonstrates batch insertion. The Contract Manager sample application contains an example of batch access in its contact adder.java source file
 
 ### Data access via intents
 Intents can provide indirect access to a content prvider. you allow the user to access data in a provider even if your application coesn't have access permissions,either by getting a result intent back from an application that has permissions, or by activiating an application that has permissions an d letting the user do work in it. 
 
 ### Getting acces s with temporary permissions. 
 
 You can access data in a content provider, even if you don't have the proper access permissions, by sending an intent to an application that does have the permissions and receiving back a result intent containing "URI" permissions. These are permissions for a specific content URI that last until the activity that receives them is finsihed. The application that has permanent permissions grants temporary permissions by setting a flag in the reuslt intent: 
 
 - Read permission: FLAG_GRANT_READ_URI_PERMISSION
 - Write permission: FLAG_GRANT_WRITE_URI_PERMISSION
 
Note: These flags don't give general read or write access to the provider whose authority is contained in the content URI. THe access is only for th  URI itself
 
When you send content URIs to another appp, include at least one of these flags. The flags provide the folllowing capabilities to any app that receives an intent and targets Android 11 

A provider defines URI permissions for content URIs in its manifest, using the anrdorid:grantUriPErmission attribute of the <provider> element, as well as the <grant-uri-permission> child element of the <probicer> elemetn. THe URI permissions mechanism is explaned in more detail in the PErmissions overview guide.
  
For exlample, you can retrieve data for a contact in the Ctonacts Provider, even if you don't have the READ_CONTACTS permission.  You might want to do this in an application that sends e-greetings to a contact on their birthday. Instead of requesting Read_contacts, which gives you access to all of the user's contacts and all of their information, you prefer to let the user control which contacts are used by your application. To do this, you use the following process;

1) Your application sends an intent containging the action ACTION_PICK and the "contacts" MIME type CONTENT_ITEM_TYPE, using the method startActivityForREsult().
2)Becuase this intent matches the intent filter for the People app's selections' activity, the activity ewill come to the foreground. 
3) In the selectionactivity, the user selects a contact to update. When this happens, the selection activity calls setResult(resultcode, intent) to set up an intent to give back to your application. The intent contains the content URI of the contact the user selected, and the "extras" flags FLAG_GRANT_READ_URI_PPERMISSION. These flags grant URI permission to your app to read data for the contact pointed to by the contnet URI> THe selection activity then calls finish() to retun control to your application
4)Your activity retuns to the foreground, and the system calls your activity's onActivityResult() method. This method receives the result intent created by the selection activity in the Peopel app
5) With the content URI from the result intent, you can read the contact's data from the Contacts Provider, even though you didn't' request permananet read access permission to the provider in tyour manifest. You can then get the contact's birthday infromation or their email addresss and then send the e-greeting. 

### Using another application 
Asimple way to allwo the user to modify data to which you don't have access permission is to activate an application that has permissions and let the user do the work there. 

For example, the Calendar application accepts an ACTION_INSERT intent, which allows ou to activate the application's insert UI. YOu can pass "extras" data in the intent, which athe application uses to pre-populate the UI. Becuase recurring events have a complex syntax, the preferred way of inserting events in to the Calendar provider is to acitvate the Calendar app with an ACTION_ISNERT and then let the user insert the even there. 

### Displaying data using a helper app
If your application does have access permissions, you still may want to use an intent to dipsplay data in another application. For example, the Calendar application accepts an ACTION_VIEW intent, which displays a particular date or event. THis allows you to display calendar information without having to create your own UI. To learn more about this feature, see the Calendar PRovider guid. 

The application to which you send th eintent doesn't have to be the application associated with the provider. Fore example, you can retieve a contact from the Contact Provider, then send an ACTION_VIEW intent contianing the content URI for the contat's image to an image viewer. 

## contract classes
A contract class defines constants that help applications work with the content URIs, column names, intent actions, and other features of a conetent prvoider. COntact classes are not included automatically with a provider; the provider developer has define them and then make them available to other developers. Many of the providers included with the Android platform have corresponding contaract classes in teh package android.provider. 

For example, the User Dictioanry PRovider has a contaract class UserDictionary containgin content URI and column name constants. The content URI for the "words" table is dfined in the constant UserDictionary.Words.CONTENT_URI. The UserDictionary.Words class also contains column name constants, which are used in teh example snippets in this guide. For example a query projection can be defined as.

```
val progjection : Array<Stirng> =  arrayOf (
  UserDictionary.Words._ID
  UserDictionary.Words.WORD
  UserDictionary.Words.LOCALE
  )
```
Anotehr contract class is ConstactsContract for the Contacts Provider. The reference documentation for this class includes example code snippets. One of its subclasses, ContactContracts.Intetns. Insert, is a contract class that contains constants for intents and intent data. 


## MIME TYpe reference
Content providers can return standard MIME media types. or custom MIME type strings, or both
MIME types have the fomrat
```
type/subtype
````

For example, the well-known MIME type text/html has the tex type and the html subtype. If the prvoider returns this type of a URI, it means taht a query using that URI will return text containing HTML tags.

Custom MIME type strings, also called "bendor-specific"MIME types have more complex type and subtype values. 
```
vnd.android.cursor.dir
```
for multipel rows, 
```
vnd.android.cursor.item
```
for a single row. 

The subtype is provider -specific. The android built-in providers usually have a simple subtype. For example, when the contacts applicationcreates a row for a telephone number, it sets the following MIME type in the row
```
vnd.android.cursor.item/phone_v2
```
