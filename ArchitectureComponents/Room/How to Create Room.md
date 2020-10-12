
## 1) Add dependency
```
// Top of build.gradle file
apply plugin: 'kotlin-kapt'

dependencies {
  def room_version = "2.2.5"

  implementation "androidx.room:room-runtime:$room_version"
  kapt "androidx.room:room-compiler:$room_version"

  // optional - Kotlin Extensions and Coroutines support for Room
  implementation "androidx.room:room-ktx:$room_version"

  // optional - Test helpers
  testImplementation "androidx.room:room-testing:$room_version"
```
}

## 2) Create an Entity
Create a new kotlin class, This class descibes the Entity Which represents the SQLite table, Each property in the class represents a column in the table. Room will use these properites to create the tabel and instantiate obejcts from rows in the database. 


@Entitiy: declares that the class is an entity

@PrimaryKey: The entry into the table, every Entity needs a primary key. 
PrimaryKey can autogenerate "@PrimaryKey(autoGenerate = true) val id: Int"

@ColumnInfo: Specifies the name of the column in the table.
```
@Entity(tableName = "word_table")
data class Word(@PrimaryKey @ColumnInfo(name = "word") val word: String)
```

## 3) Create a DAO
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

```

## 4) Database
```
// Annotates class to be a RoomDatabase with table (entity).
// @Database: Use the parameters to declare the entities that belong in the database and set the version number. 
@Database(entities = arrayOf(Word::class), version = 1 exportSchema = false)
abastract class WordRoomDatabase : RoomDatabase() {

    // Database exposes the DAOs through an abstact getter, for each DAO
    abastract fun wordDao(): WordDao
    
    // Populate the database
    private class WordDatabaseCallback(
      private val scope: CoroutineScope
    ) : RoomDatabase.Callback() {
      override fun onOpen(db: SupportSQLiteDatabase) {
        super.onOpen(db)
        INSTANCE?.let { database -> 
          scope.launch {
            var wordDao = database.wordDao()
            
            // Delete all content
            wordDao.deleteAll()
            
            // Add sample words.
            var word = Word("Hello")
            wordDao.insert(word)
            word = Word("World!")
            wordDao.insert(word)

            word = Word("TODO!")
            wordDao.insert(word)
          }
        }
      }
    }
    
    companion object {
      // Singleton prevents mutlipele instances of database opening at the same time. 
      @Volatile
      private var INSTNACE: WordRoomDatabase? = null
      
      fun getDatabase(context: Context): WordRoomDatabase {
        val tempInstnace = INSTANCE
        if(tempInstnace != null) {
            return tempInstance
        }
        synchronized(this) {
          val instnace  = Room.databaseBuilder(
            context.applicationContext,
            WordRoomDatabase::class.java
            "word_database"
          ).build()
        INSTANCE = instance
        return instnace
        }
      }
    }
}
```

## 5) ViweModel
```
class MyViewModel(application: Application) : AndroidViewModel(application) {
  // Reference to word reposioty. 
  private val reposiory: WordReposioty
  
  // using liveData and caching what getAlphabetizeedWords returns has several benefits
  // - observer will only update the UI when the data actually changes
  // - repository is completely separated from teh UI through the ViewModel. 
  val allWords: LiveData<List<Word>>
  
  init {
    val wordsDao = WordRoomDatabase.getDatabase(application).wordDao()
    reposiotory = WordRespository(wordDao)
    allWords = Repository.allWords
  }
  
  /**
   * Launching a new coroutine to insert the data in a non-blocking way
   */
   fun insert(word: Word) = viewModelScope.launch(Dispatchers.IO) {
    repository.insert(word)
  }
}
```
In order to use ViewModelScope you will need to add the following dependency
```
    // ViewModel scope
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$rootProject.archLifecycleVersion"
```
