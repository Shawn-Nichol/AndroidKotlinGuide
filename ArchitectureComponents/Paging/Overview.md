# What is paginging
The Paging library helps you load and display small chunks of data at a time. loading partial data on demand reduces usage of network bandwidth and system resources. 

## Setup import dependencies
```
dependencies {
  def paging_version = "2.1.2"

  implementation "androidx.paging:paging-runtime:$paging_version" // For Kotlin use paging-runtime-ktx

  // alternatively - without Android dependencies for testing
  testImplementation "androidx.paging:paging-common:$paging_version" // For Kotlin use paging-common-ktx

  // optional - RxJava support
  implementation "androidx.paging:paging-rxjava2:$paging_version" // For Kotlin use paging-rxjava2-ktx
```

## PagedList
The paging library's key component is the PagedList class, which loads chunks of your app's data, or pages. As more Data is needed, it's paged into the existing PagedList object. If any loaded data or RxJava2-based object as PagedList Objects are generated, your app's UI preseents their contents all while respecting your UI controllers lifecycles.

The followign cod snippet shows how you can configure your app's view model to load and present data using a liveData holder of PagedList objects

```
class ConcertViewModel(concertDAo: ConcertDao) : ViewModel() {
  val concertList: LiveData<PagedList<Concert>> ==
     concertDao.concertsByDate().toLiveData(pageSize = 50)
}
```

Data
Each instance of pagedlist loads an up-todate snapshot of your app's data from its corresponding DataSource object. Data flowws from your app's backend or database into the PagedList object

The following example uses the Room persistence Library to organize your app's data, but if you want to store your data using another means, you can also provide your own data source facotry. 
```
@Dao
interface ConcertDao {
    // The Int type parameter tells Room to use a PositionalDataSource object.
    @Query("SELECT * FROM concerts ORDER BY date DESC")
    fun concertsByDate(): DataSource.Factory<Int, Concert>
}
```


## UI
THe pagedList class works with a PagedListAdapter to load items into a RecyclerView. These classes work toether to fetch and display content as it's loaded, prefetching out-of-view contnetn and animating content changes. 

