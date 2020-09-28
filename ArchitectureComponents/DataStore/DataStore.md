Jetpack DataStore is a data storage solution that allows you to store key-value pairs or typed objects iwth protocol buffers. DataStore uses kotlin coroutines and flow to store data asynchronously consistently and transactionally. 

Ifyou're currently using SharedPreferences to store data, consider migrating to DataStore instead. 

Note: If you need to support large or comples datashets, partial updates, or referential integrity consider using Room instead od DataStore. DataStore is ideal for small, simple datasets and does not support partial updates or referential integrity.

## Preferences DataStore and Proto DataStore
DataStore provides two different implementations: Preferences DataStore and Proto DataStore. 
- Preferences DataStore stores and access data using keys. This implemenetations does not require a predefined schema, and it does not provide type saftey. 
- Proto DataStore stores data as instances of a custom data type. This implementation requires you to define a schema using protocols buffers, but it provides type safety.

## Setup
To use Jetpack DataStore in your app, add the following to your gradle file
```
dependencies {
  // Preferences DataStore
  implementation "androidx.datastore:datastore-preferences:1.0.0-alpha01"

  // Proto DataStore
  implementation "androidx.datastore:datastore-core:1.0.0-alpha01"
}
```

Store key-value pairs with Preferences DataStore
The Preferences DataStore implementation uses the DtaStore and Preferences classes to persist simple key-value pairs to disk. 

## Create a Preferences DataStore
Use the Context.cretaeDataStore() extension function to create an instance of DataStore<Preferences>. The mandatory name parameter is the name of the Preferences DataStore. 

```
val dataStore: DataStore<Preferences> = context.cretaeDataStore (name = "settings")
```

## Read from a Preferences DataStore
Becuase Preferences DataStore does not use a predefined schema, you must use preferenceskey() to define a key for each value that you need to store in the DataStore<Preferences> instance. Then, use the DataStore.data property to expose the appropriate stored value uisng a FLOW
```
val EXAMPLE_COUNTER = preferencesKey<Int>("example_counter")
val exampleCounterFlow: Flow<Int> = dataStore.data
  .map { preferences -> 
    // No Type safety
    preferences[EXAMPLE_COUNTER] ?: 0
}
```

## Write to a PReference DataStore
Preferences DataStore provides an edit() function that transactionally updates the data in a DataStore. The function's transform parametere accepts a block of code where you can update the values as needed. All o fthe code int he transform block is treated as a single transaction
```
suspend fun incrementCounter() {
  dataStore.edit {setting -> 
    val currentCounterValue = settings[EXAMPLE_COUNTER] ?: 0
    settings[EXAMPLE_COUNTER] = currentCounterValue + 1
```

## Stored typed objects with Proto DataStore
The Proto DataStore implementation uses DataStore and protocol buffers to persist typed objects disk. 

## Define a schema
Proto DataStore requires a predefined schema in a proto file in the app/src/main/proto/ directory about definning a proto schema, see the protobuf laanguage guide.
```
syntax = "proto3";

option java_package = "com.example.application";
option java_multiple_files = true;

message Settings {
  int example_counter = 1;
}
```

Note: the class for your stored objects is generated at compile time from the message defined in the proto file. Make sure you rebuild your project. 

## Createa a Proto DataStore
There are two steps involved in creating a Proto DataStore to store your typed objects:
1) Define a class that implements SErializer<T>, where T is the type defined in te proto file. This serializer class tells DataStore how to read and write your data type. 
2)Use the Context.createDataStore() extension function to create an instance of DataStore<T>, where T is the type defined in the proto file. The filename parameter tells DataStore which file to use to store the dta and the serializer paramaeter tells DataStore the name of the serializer class 

```
Object settingsSerializer : Serializer<Settings> {
  override fun readFrom(input: InputStream): Settings {
    try {
      return Settings.parseFrom(input)
    } catch (exception: InvalidProtocolBufferException) {
      throw CorruptionException("cannot read proto.", exception)
    }
  }
  
  overrid fun writeTo(
    t: Settings, 
    output: OutputStream) = t.writeTo(output)
  }
  
  val settings DataStore: DataStore<Settings> = context.createDataStore(
    fileName = "settings.pd"
    serializer = SettingsSerializer
  )
  
  
```

## Read from a Proto DataStore
Use DataStore.data to expose a Flow of the appropriate property from your stored object. 
```
val exampleCounterFlow: Flow<Int> = sesttingsDataStore.data
  .map { settings -> 
    // The exampleCounter property is generated fromt he proto schema
    settings.exampleCounter
  }
```

## Write to a Proto DataStore
Proto DataStore provides an updateData() function that transactionally updates a stored object. updateData() gives you the current state of the data as an instance of your data type and updates the data transactionally in an atomic read-write-modify operation
```
suspend fun incrementCounter() {
  settingsDataStore.updateData { currentSettings -> 
    currentSettings.toBuilder()
      .setExampleCounter(currentSettings.exampleCounter + 1) 
      .build()
    }
  }
```

## Use DataStore is synchronous code
Caution: aVoid blocking threads on DataStore data reads whenever posssible. Blocking the UI thread can cause ANRs or UI jank, and blocking other threads can result in deadlock. 

ONe of the primary benefits of DataStore is the asynchronous. This might be the case if you're working with an existing codebase that uses synchronous disk I/O or if you have a dependency that doesn't provide an asynchronous API.

Koltin coroutines provide the runBlocking() coroutine builder to help bridge the gap betweeen synchronous and asynchronous code. You can use runBlocking(0 to read data from DataStore synchronously. The following code blocks the calling thread until DataStore retuns data.
```
val exampleData = runBlocking {dataStore.data.first()( }
```

Performing synchronous I/O operations on the UI thread can cause ANRs or UI jank. You can mitigate these issues by asynchronously preloading the data from DataStore:
```
override funonCreate(savedInstanceState: Bundle?) {
  lifecycleScope.launch {
    dataStore.data.first()
    // You should also handle IOExceptions here.
    }
  }
} 
```

This way, DataStore asynchronously reads the data and caches itin memory. Later synchronus reads using runBlockign() may be faster or may avoid a disk I?O operatioons altogether if the initial read has completed. 
