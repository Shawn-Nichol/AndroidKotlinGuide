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
The contentResolver.quer() client method
