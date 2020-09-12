# ViewModel Overview
The ViewModel calss is designed to store and manage UI-elated data in a lifecycle conscious way. The ViewModel class allows data to survive configuration chanages such as screen rotations

The android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-createa UI controller in response to certain user actions or device events that are completely out of your control if the system destroys or re-creates a UI Controller, any transient UI-related  data you store in them is created for a configuration change, the new activity has to re-fetch the list of users. For simple data, the activity can use the onSaveInstanceState() method and restore its data from the bundle in onCreate(), but this approach is only suitable for small amount sof data that can be serialized then deserialized, not for poetntially lare amounts of data like a list of users or bitmaps. 

Another problem is that UI controllers frequently need to make sasynchronous calls that may take some time to treturn. The UI controllers frequetnly need to makes asynchronous calls that may take some time to return. The UI controller needs to manage these calls and ensure the system cleans them up after it's destoryed to avoid potentail memory leaks. This management requires a lot of maintance, and in the case where the object is re-created for a configuration change, it's a waste of resources since the object may have to reissue calls it has already made. 

UI controllers such as activites and fragments are primarily intended to displays  UI data, react to user actions, or handle operating system communicatinos, such as permission requests. Requiring UI controllers to also be responsilbble for loading data from a database or network adds bloat to the class. Assigning exessive responsibility to UI controllers can result in a single class that tries to handle all of an app's work by itself, instead od delegating work to other classess. Assigning exceessive respo9nsibility to the UI controlers in this way also makes teseting a lot harder

It's easier  and more effiecient to separate out view data ownership from UI control logic. 

## Implement a ViewModel
