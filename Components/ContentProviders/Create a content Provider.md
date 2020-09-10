# create a content Provider 
A content provider manages access to a central reposiotry of data. you implement a prvodier as one or more classes in an Android application, along with elements in the manifest file. One of your classes implements a subclass ContentProvider, which is the interface between your provider and other applications. Although content providers are meant to make a data available to ther applications, you may of course have activites in your applicaion that aloow the user to query and modify the data managed by your provider

## Before you start buiding
1) decide if you need a content provider. You need to build a conent provider if you want to provide one or more of the following features:
- You want tooffer complex data or files ot other applications
- You want to allow users to copy complex data from your app into other apps 
- YOu want to prlvide custom search suggestions using the search framework. 
- You want to expose your application data to widgets
- You want to implement the abastractThreadSyncAdapter, CursorAdapter, or CursorLoader classes. 

You don't need a provider to use databases or other types of persistent storage if the use is entirely within your own application and you don't need any of the features listed above. Instead, you can use oneof the storage systems desciebed on the Saving App Data page. 


## Build Provider
1) design the raw storage for your data. Content provider offers data in two ways:
File data
Data that normally goes into files, such as photos, audio, or videos, Store the files in your application's private space. In response to a request for a file from another application your provider can offer a handle to the file. 

Structured data
Data that normally goes into a database, array, or similar structure. Store the data in a form that's compatible with tables of rows and columns. A row represents an entity, such as a person or an item in inventory. Acolumn represents some data for the entity, such as a person's naem or an item's price. Acommon way to store this type of data is in an AQLite database, but you can use any type of persistent storage. To learn more about the storage types avaiable in the Android system, see the section desgining data storage. 

2) Define a concrete implementation of the ContentPRovider class and its required mthods. This calss is the interface between oyur data and the rest of the Android system. For more information about this calss, see the section impelemnting the contentPRovider class. 

3) Define the provider's authority string, its content URIs, and column names. If you want the provider's application to hadnle intents, also define tinent actions,extras data and flags. Also define the permissions that you will require for applications that want to access your data. You should consider defining all of these values as constants in a separate class; later, you can expose this class to other developers. For more information about content URIs,see the section Desigingin COntent URIs. For more information about intents and data access

4) Add other optional pieces, such as sample data or an implementation of AbastractThreadedSyncADapter that can synchornize data between the provider and cloud-based data. 

## Designing data storage
A content provider is the interface to data saved in a structured format. Before you create the interface, you must decide how to store the data. You can stggore the data in any form you like, and then design the intterfface to read and write the data as necessary. 

- If youare working with structured data then consider either a relational database such as SQLite, or a non relational key-value datastore such as LevelDB. If you are working with unstructred data such as audio, image or video media then consider storing the data as files. You can mix and match several different types of storage, and expose them using a single content provider if necessary. 

- The Android system can interact with the Room persistence lbrary, which provicdes access to the SQLite database API that Android's own providers use to store table-oriented data. To create a database using this library, instantiate a subclass of RoomDatabase, as descibed in the Room persisence library guide. 

Remember that youd on't have to use a database to implement your repsoitory. A provider appears externally as a set of tables, similar to a relational database, but this is not a requirement for the provider's internal implementation. 

- For storing file data, Android has a variety of file-oriented APIs. To learn more about file storage,read the topic Data storage. If you're designing a prvodier that offers media-related data such as music or viddeos, you can have a provider that combines table data and files. 

- In rare cases, you might benefit from implementing more than one content provider for a single application. For example, you might want to share some data with a widget using one content provider, and expose a different set of data for shaing with other applications. 

- For working with network-based data, use classes in java. net and android.net. YOu can also synchronize network-based data to a local data store such as a database, and then offer the data as tables or files. The basic Sync Adapter sample application demonstartates this type of synchronization .

Note; If you make change to your repository that isn't backwards-compatiable, you need to mark the repository with a new version number. You also need to increase the version number for your app that implements the new content provider. Making this chagne prevents sytem downgrades from causing the  system to crash when it attempts to reinstall an app that has anincompatible content prvoider. 

## Data design considerations
Here are some tips for designing your provider's data structure:
- Table data should always have a primiary key column that the provider maintains as a unique numeric value for each row. you can use this value to link the row to related rows in other tables(using it as a foreign key. Although you can use any name for this column, using BaseColumns.ID is the best coikce, becuase linking the results of a provider query toa ListView requiers on of the retrieved colus to have the name ID. 

- If you want to provide bitmap images or other very large pieces of ile-oriented data, store the data in a file and then provide it indierectly rather than storing it directly in a table. If you do this, you need to tell users of your provider that they need ot use a ContentResolver file method to access the data

- Use the Binary Larg Object (BLOB data type to store data that varies in size or has a varying structure. For example, you can use a BLOB column to store a protocl buffer or JSON structure. 

You can also use a BLOB to implement a schema-indepemendent table. In this type of table, you define a primary key column, a MIME type column, and one or more generic columns as BLOB. The meaning of the data in the BLOB columns i s indicated by the value in the MIME type column. This allows you to store differen trow types inthe same table. The contacts Provider's data table Contact Contract.Data is an example of a schema-indepemenddt table. 

## Designing content URIs
A content URI is a URI that identifies data in a provider. Content URIs include the symbolic name of the entire provider (its autority) and a name that points to a table or file. THe optional id part points to an individual row in a table. Every data access method of ContentProvider has a content URI as an argument; this allows you to determine the table, row or file to access. 

The basics of content URIs are described in the topic Content provider baseics. 

### Desiging an authority
A provider usually has a single authority, which serves as its Android-internal name. To avoid conflicts with other providers, you should use Internet domain ownership (in reverse) as the basis of your provider authority. BEcuase this recommendation is also true for Android package names, you can define your provider authority as an extension of the name of the package containg the provider. For example, if you Android package name is com.example.<appname>, you should give your prooivder the authority com.example.<appname>.provider.
  
### Designing a path structure.
Developers usually create content URIs from the authroity by appending paths that point to individual tables. For example, if you have two tables table1 and table2, you combine the authority from the prevois example to yield the content URIs com.example.<appname>.provider/table1 and com.example.<appname>.provider\table2. Paths aren't limited toa single segment and there doesn't have to be a table for each level of the path. 
  
### Handling content URI IDs
By convention, providers offer access to a single row in a table by acceptinga content URI with an ID value for the row at the end of the URI. Also by convention, providers match the ID value to the table's ID column, and perfrom the requested access against the row that matches. 

This convention facilitates a common design pattern for apps accessing a provider. the app deos a query against he provider and displays the resultCursor in a ListView using a CursorAdapter. The definition of CursorAdapter requires one of the columns in the Cursor to be ID. 

The user thenn picks one of the diplayed rows from the UI in order to look at or modify the data. THe app gets the corresponding row from the Cursor backing the ListView, gets the ID value for this row, appends it to the content URI, and sends the access request to the provider. THe provider can then do the query or modification against the exact row the user picked. 

## Content URI patterns. 
To help you choose which aaction to take for an incoming content URI, the provider API includes the convenince class UriMatcher, which maps content URI "patterns" to integer values. You can use the integer valeus in a switch statement that chooses the desired action for the content URI or URIs that match a partticular pattern. 

A content URI pattern matches content URIs using wildcard characters: 
- * Matches a stirng of any vlid characters of any length.
- # Matches a stinrg of numeric characters of any length. 

As an example of designing and coding content URI handling, consider a provider with the authority com.exmaple.app.provider that reconginzes the following content URIs pointing to a table. 
content://com.example.app.provider/table1: A table called table1.
content://com.example.app.provider/table2/dataset1: A table called dataset1.
content://com.example.app.provider/table2/dataset2: A table called dataset2.
content://com.example.app.provider/table3: A table called table3.
The provider also recognizes these content URIs if they have a row ID appended to them, as for example content://com.example.app.provider/table3/1 for the row identified by 1 in table3.

The following content URI patterns would be possible:

content://com.example.app.provider/*
Matches any content URI in the provider.
content://com.example.app.provider/table2/*:
Matches a content URI for the tables dataset1 and dataset2, but doesn't match content URIs for table1 or table3.
content://com.example.app.provider/table3/#: Matches a content URI for single rows in table3, such as content://com.example.app.provider/table3/6 for the row identified by 6.
