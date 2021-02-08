- Persistence tests help keep the users data safe. 
- Statefulness can make persistence tests difficult to write.
- In-memory, use `in-memory` to create repeatable tests with the database, this will prevent data from being saved to your database and removed from your database.
- You need to include both set up @Beofre and tear down @After with persistence tests. 
- Be careful to test the code and not library
- Sometimes you need to write brocken code for the test to fail.
- You can use factories to create tests data for reliable, repeaatable tests. 

```
@RunWith(AndroidJunit4::class) 
class MyDaoTest {

  // Instant taskExecutorRule is used to swap out the executor and replace  it with a syncrounous one. 
  // This will make sure that, when using livedata, it all runs syncrounously in tests. 
  @get:Rule
  var instantTaskExecutorRule = InstantTaskExecutorRule()
  
  private lateinit var myDatabase: MyDatabase
  private lateinit var wishlistDao: MyDao
  
  
  @Before
  fun initDb() {
    // Use a Room builder to create an in-memory Database. Informmation stored in an in-memory database disappears when the tests finsihes. 
    myDatabase = Room.inMemoryDatabaseBuilder(
      InstrumentationRegistry.getInstrumentation.context,
      MyDatabase::class.java).build()
      
      // Use database to get DAO
      myDao = myDatabase.myDao()
  }
  
  @After
  fun closeDb() {
    myDatabase.close()
  }
  
  @Test
  getAllReturnsEmptyList() {
    val testObserver: Observer<List<MyList>> = mock()
    wishlistDao.getAll().observeForever(testObserver)
    verify(testObserver).onchanged(emptyList())
  }
  
  
  @Test
  fun saveMyListSaveData() {
    val myList1 = ListFactory.makeList()
    val mylist2 = ListFactory.makeList()
    
   myDao.save(myList1, myList2)
   
   // Mock object used to call
   val testObserver: Observer<List<MyList>> = mock()
  
   myDao.getAll().observeForever(testObserver)
    
   // Create an ArguementCaptor to capture the value in onChanged(). Using an arguement captor from Mockito allows you to make more complex assertions on a value than equals. 
   val listClass = ArrayList::Class.java as Class<ArrayList<MyList>>
   val arguemntcaptor = ArgumentCaptor.forClass(listClass)
   
   // Test that the result from the databasee is not empty. At this point you care that data was saved. 
   verify (testObserver).onChanged(argumentCaptor.captore())
   
   assertTrue(arguemtnCaptor.value.size > 0)
  }
  
  
  @Test
  fun getAllRetrievedsDate() {
    val myList1 = ListFactory.makeList()
    val myList2 = ListFactory.makeList()
    mydao.save(myList1, myList2)
    
    val testObserver: Observer <List<MyList>> = mock()
    myDao.getAll().observerveForever(testObserver)
    
    val listClass = ArrayList::class.java as Class<Array<MyList>>
    
    val argumentCaptor = ArgumentCaptor.forClass(listClass)
    
    verify(testObserver).onChanged(arguementCaptor.capture())
    
    val capturedArguemtn = arguemntCaptor.value
    
    assertTrue(capturedArguemnt.containsAll(listOf(myList1, myList2)))
  }
  
  @Test
  fun findDataById() {
    val myList1 = ListFactory.makeList()
    val myList2 = ListFactory.makeList()
    myDao.save(myList1, myList2)
    
    val testObserver: Observer <list<MyList>> = mock()
    myDao.getAll().observeForever(testObserver)
    
    val listClass = ArrayList::class.java as Class<Array<MyList>>
    
    val arguementCaptor = ArgumentCaptor.forClass(listClass)
    
    verify(testObserver).onChanged(arguemntCaptor.capture)
    
    val capturedArguemnt = arguemntCaptor.value 
    assertTrue(capturedArguemnt.containsAll(listOf(myList1, myList2))
  }
  
  @Test
  fun findByIdretrievesCorrectData() {
    val myList1 = ListFactory.makeList()
    val myList2 = ListFactory.makeList()
    myDao.save(myList1, myList2)
    
    val testObserver: Observer<MyList> = mock()
    
    myListDao.findById(myList2.id).observerForever(testObserver)
    verify(testObserver).onChanged(wishlist2)
  }
  
}
```


ListFactory
```
object ListFactory {
  private fun makeRandomString() = UUID.randomUUID().toString()
  
  private fun makeRandomInt() = ThreadLocalRandom.current().nextInt(0, 1000 + 1) 
  
  fun makeList(): MyList {
    return MyList(makeRandomString(), 
      listOf(makeRandomString(), makeRandomString()),
      makeRandomInt())
  }
}
```
