# Room Single Item Query

#Dao
```
@Dao
interface MyDao {
  @Query("SELECT * FROM my_table Where rowId = :id")
  suspend fun getSingleItemId(id: Int) : Player
}
```

Repository
```
class MyReposiotry(private val myDao: MyDao) {
  @Dao
  interface MyDao {
    supsend fun getItem(id: Int) MyItem {
      return MyDao.getSingleItem(id)
    }
}
```

ViewModel
```
class MyViewModel(application, Application) : AndroidViewModel(application) {
  
  lateinit var item: MyItem
  ...
  
  
  
  fun getItem() = viewModelScope.launch(Dispatchers.IO) {
    myItem = repository.getItem(position)
  }
}
```
