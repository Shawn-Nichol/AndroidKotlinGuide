
## 1) Create an Entity
Create a new kotlin class, This class descibes the Entity Which represents the SQLite table, Each property in the class represents a column in the table. Room will use these properites to create the tabel and instantiate obejcts from rows in the database. 


@Entitiy: declares that the class is an entity

@PrimaryKey: The entry into the table, every Entity needs a primary key. 
PrimaryKey can autogenerate "@PrimaryKey(autoGenerate = true) val id: Int"

@ColumnInfo: Specifies the name of the column in the table.
```
@Entity(tableName = "word_table")
data class Word(@PrimaryKey @ColumnInfo(name = "word") val word: String)
```

## 2) Create a DAO
In the Data Access Object you specify SQL queries and associate them with method calls. The compiler checks the SQL and generates queries from convenience annotation for common queris, such as @Insert

Room has coroutines support, allowing queries to be annotated with the suspend modifier and then called form a coroutine or from another suspension function


```

// DAO must be an interface or abstract.
// @Dao identifies the class as DAO class for Room.
@Dao
interface WordDao {
  
  // @Query that returns a list of words. 
  @Query("SELECT * from word_table ORDER BY word ASC")
  // If you use LiveData Room will generate all the necessary code required to upate the livedata when the database is updated. 
  fun getAlphabetizedWords(): LiveData<List<Word>>
  
  // @Insert is special DAO annotation where you don't have to provide any SQL
  // OnConflictStrategy decides what to do if there is a conflict. 
  @Insert(onConflict = OnConflictStrategy.IGNORE)
  // Declares a suspend function to insert one word. 
  suspend fun insert(word: Word)
  
  // @Query: requires that you provide a SQL query as a string
  @Query("DELETE FROM word_table")
  // Declares a  supsend function to delete all words. 
  suspend fun deleteAll()
}


## 3) Database
```
