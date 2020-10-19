## Use coroutine with LiveData
When using LiveData, you might need to calculate values asynchronously. For example, you might want to retrieve a user's prefences and serve them to your UI. In these cases, you can use the liveData builder function to call a suspend function, serving the result as a LiveData object. 

In the example below, loadUser() is a suspend function declared elsewhere. use the LiveData builder function to call loadUser() asyncrhonously, and then use emit() to emit the result:

```
val user: LiveData<User> = liveData {
  val data = database.loadUser() // LoadUser is a supsend function.
  emit(data)
}
```

The liveData building block serves as a structured concurrency primitive between coroutines and LiveData. The code block starts executing when LiveData becomes active and is automatically canceled after a configurable timeout when the LiveData becomes inactive. If it is canceled before completion, it is restarted if the LiveData becomes active again. If it completed successfully in a previous run, it doesn't restart. Note that it is restarted only if canceled automtaicly. If the block is canceled for any other reason it is not restarted. 

You can also emit mutiple values from the block. Each emit() call suspends the execution  of the block until the LiveData value is set on the main thread. 
```
val user: LiveData<Result>  = liveData {
  emit(result(Result.loading()) 
  try {
    emit(Result.success(fetchUser()))
  } catch (ioException: Exception) {
    emit(Result.error(ioException))
  }
}
```

You can also combine liveData with Transformations, as shown in the following
```
class MyViewModel : ViewModel() {
  private val userId: LiveData<String> = MutableLiveData()
  val user = userId.switchMap { id -> 
    liveData(context = viewModelScope.coroutineContext + Dispatchers.IO) {
      emit(database.loadUserById(id)) 
    }
  }
}
```

You can emit multiple values from a LiveData by calling the emitSource() function whenever you want to emit a new value. Note that each call to emit(0 or emitSource(0 removes the previously added source. 
```
class UserDao: Dao {
    @Query("SELECT * FROM User WHERE id = :id")
    fun getUser(id: String): LiveData<User>
}

class MyRepository {
    fun getUser(id: String) = liveData<User> {
        val disposable = emitSource(
            userDao.getUser(id).map {
                Result.loading(it)
            }
        )
        try {
            val user = webservice.fetchUser(id)
            // Stop the previous emission to avoid dispatching the updated user
            // as `loading`.
            disposable.dispose()
            // Update the database.
            userDao.insert(user)
            // Re-establish the emission with success type.
            emitSource(
                userDao.getUser(id).map {
                    Result.success(it)
                }
            )
        } catch(exception: IOException) {
            // Any call to `emit` disposes the previous one automatically so we don't
            // need to dispose it here as we didn't get an updated value.
            emitSource(
                userDao.getUser(id).map {
                    Result.error(exception, it)
                }
            )
        }
    }
}
```
