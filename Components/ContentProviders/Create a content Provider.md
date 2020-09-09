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
