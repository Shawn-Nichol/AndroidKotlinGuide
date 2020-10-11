- PagingData: A container for paginated data. Each refresh of data will have s eparate corresponding PaginingData. 

- PaginginSource: A PagingSource is the base class for loading snapshots of data inot a stream of PagingData. 

- Pager.Flow: builds a Flow<PagingData>, based on a PagiingConfig and a function that defines how to construct the implemented PagingSource.

- PagingDataAdapter: A RecyclerVeiw.Adapter that presents PagingDaa in a RecyclerView. the PaginingDataAdapter can be connected to a Kotlin flow, a LiveData, RxJava Flowable, or an RXJava Observable. The pagingDataAAdapter listens to internal PagingData loading events as pages are loaded and uses DiffUtil on a background thread tompute fine-grained updates as updated contents is received in the form of new PagingData objects

- RemoteMediator -helps implement pagination from network and database. 
