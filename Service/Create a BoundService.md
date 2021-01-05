# How to create a Bound Service. 

## Create service class
Create a class that extends service
```
class MyBoundService : Service() {
  
}
```

## Over Methods

```
class MyBoundService : Service() {
  
  // Binder given to the clients
  private val binder = LocalBinder()
  
  // Random number generator
  private val mGenerator = Random()
  
  
  // Method for clients
   val randNumber: Int
    get() = mGenerator.nextInt(100)
  
  
  /**
   * Class used for the lient Binder. Becusae we knwo this service always runs in the same process as its clients, we don't need to deal with IPC. 
   **/
   inner classd localBinder : Binder() {
    // Returns this instance of LocalService so clients can call public methods
    fun getService(): MyBoundService = this@MyBoundService
   }
  
  override fun onBind(intent: Intent) {
    return binder
  }
}
```

## Bind service
Bind service to activity or fragment make sure to unbind when closed to avoid memory leaks. 
```
// Fragment

fun boundService() {
  Intent(requireContext(), MyBoundService::class.java).also {
    requireContext().bindService(it, connection, Context.BIND_AUTO_CREATE)
  }
}
```

## Manifest
The service will need to be added to the Manifest in order for it to load. 
```
<application>

  <service android:name=".MyBoundService"/>
</application>
```
