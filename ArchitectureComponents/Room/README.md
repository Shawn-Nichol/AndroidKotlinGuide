# What is Room
The room persistence library provides an abstaction layer over SQLite to allow for more robust database access while harnessing the full power of SQLite

The library helps you create a cache of your app's data on a device that's running your app. This cache, which serves  as your app's single source of truth, allows users to view a consistent copy of key information within your app, regardless of whether users have an internet connection 

## Glossary

### Entity
`@Entity:` The entity which represenets the SQLite table, Each property inthe class reprsents a column in the table. Room will use these properties to create the table and instantiate objects form rows in the database.

`@Primary Key:` The entry into the table, every Entity needs a primary key.
`@ColumnInfo:` Specifies the name of the colun in the table. 

### DAO
Data Access Object you specify SQL  queries and associate them iwth method calls. The compiler checks the SQL and generates queries form convenience annotation for common queries,

`@Insert:` Allows you to define methods that insert their parametersw into the appropariate table.
`@Update:` Allows you to tdefine methods that update specific rows in a database.
`@Query:`Allows you to write SQL statements and expose them as DAO methods. Use this to query data in the database. 
`@Delete:` Allows you to define methods that delete specific rows from the database.
