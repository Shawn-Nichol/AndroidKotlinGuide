# Dagger Setup
How to setup dagger, this example will pass in application context and a Module that provides a database and DAO for use with ROOM.

## Dependencies
```
dependencies {
    // Room
    def room_version = "2.2.6"
    implementation "androidx.room:room-runtime:$room_version"
    kapt "androidx.room:room-compiler:$room_version"
    implementation "androidx.room:room-ktx:$room_version"
}
```

## AppComponent Interface
```
@Component(modules = [MyModule::class])
interface AppComponent {
    // Provides a way to pass context to the graph.
    @Component.Factory
    interface Factory {
        fun create(@BindsInstance context: Context): AppComponent
    }

    // Class that can be injected
    fun  inject(activity: MainActivity)
}
```

## Application Class
The Application class name will need to be added to the application class in the Manifest
```
class MyApplication : Application() {
    // Instance of the AppComponent will be used by all components in the project.
    val appComponent: AppComponent by lazy {
        // Passes the application context to the Dagger graph.
        DaggerAppComponent.factory().create(applicationContext)
    }
}
```

## Modules
```
@Module
class MyModule {
    companion object {
        val applicationScope = CoroutineScope(SupervisorJob())

        @Provides
        @Singleton
        fun provideDatabase(context: Context): WordRoomDatabase {
            return WordRoomDatabase.getDatabase(context, applicationScope)
        }

        @Provides
        @Singleton
        fun provideDao(database: WordRoomDatabase): WordDao {
            return database.wordDao()
        }
    }
}
```

## Inject class into graph
```
class WordRepository @Inject constructor(private val wordDao: WordDao) {
```

## Setup Activity

```
    @Inject
    lateinit var viewModel: MainViewModel

    @Inject
    lateinit var rvAdapter: RVAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        // must be called before super
        (application as MyApplication).appComponent.inject(this)
        super.onCreate(savedInstanceState)
```
