# Load and display paged data

The paging library provides powerful capabilities for loading an ddisplaying paged data from a larger dataset. This guide demonstrates how to use the pagining library to setup a stream of paged data from a a network data source and ddisplay it in a recyclerView.

## Define a data source. 
The first step is to define a Pagingsource implementation to identify the data source. The PagiingSource API class includes the load() method, which you must override to indicate how to retrieve paged data from the corresponding data source. 

Use the PagingSource class directly to use Kotlin coroutines for async loading. The paging library also provides class to suppport other asyn frameworks
- To use RxJava, implement RxPagingSource
- To use ListenableFuture from Guava, implement ListenableFuturePagingSource instead. 

## Select key and value types
Pagingsource<Key, Value> has two type parameters: Key and Value. The key defines the identifier used to load the data, and the value is the type of the data itself. For example if you load pages of User objects from the network by passing Int page numbers to Retrofit you would select Int as the key type and User as teh Value type. 

## Define the Paging Source
The follwoing example implements a pagining source that loads pages of items by page number. The key type is Int and the Value is User
```
class ExamplePagingSource(
  val backend: ExampleBackendService, 
  val query: String
): PagingSource<Int, User>() {

  override suspend fun load(
    params: LoadParmas<Int>
  ): LoadResult<Int, User> {
    try {
      // Start refresh at page 1 if undefined
      val nextPageNumber = params.key ?: 1
      val response = backend.searchUsers(query, nextPageNumber) 
      return LoadResult.Page(
        data = response.users,
        prefKey = null, // Only paging forward
        nextKey = response.nextPageNumber
      )
    } catch (e: Exception) {
      // Handle errors. 
    }
  }
}
```

A typical PagiingSource implementation passes parameters provided in its constructor to the load() method to load appropriate data for a query. In the example above, those parameters are
- backend: an instance of the backend service thatprovides the data
- query: Thesearch query to send the service indicated by backend.

The loadParams object contains information about the load operations to be perfromed. This includes the key to be loaded and the number of items to be loaded. 

The loadResult object contains the result of the load operation. LoadResult is a sealed class that takes one of the two forms, depending on whether the load9) call succeeded:
- If the laod is successful, return a LoadResult.Page object
- If the load is not successful, return a LoadResult.error object

## handles Errors
Request to load data can fail for a number of reasons, especially when loading over network. Report errors encountered during loading by returning a LoadResult.Error object from the load() method. 

```
catch(e: IOException) {
  // IOException for network failures.
  return LoadResult.Error(e)
} catch(e: HttpException) {
  // HttpException for any non-2xx HTTP status codes. 
  return LoadResult.Error(e)
}
```

## set up a stream of PagingData. 
