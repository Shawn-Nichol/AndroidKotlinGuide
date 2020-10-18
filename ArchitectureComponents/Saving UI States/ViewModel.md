## Use ViewModel to handle configuration changes. 

ViewModel is ideal for storing and managing UI-related data while the user is actively using the application. It allows quick access to UI data nad helpls you avoid refeching data from the network or disk across rotation, window resizing, and other commonly occuring configuratino changes.

ViewModel retains the data in memory, which means it is cheaper to retrieve than data form the disk or the ntwork. A ViewModel is associated with an activity (or someother lifecycleowner) it stays in memory during a configuration change and the system automatically associates the ViewModel with the new activity instance that results from the configuration chagne. 

ViewModels are automatically destoryed by the system when your user backs out of your activity or fragment or if you call finish(), which means the state will be cleared as the user expects in these scenarios. 

Unlike savedInstanceStatee, ViewModels are desotyed during a system-initiated process death. This is why you should use ViewModel objects in combination with onSavedInstanceState or someother disk persistence, stashing identifiers in savedInstanceStae to help ViewModels reload the data after system death. 

