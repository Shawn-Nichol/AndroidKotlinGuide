## OnSaveInstanceState

Use onSaveInstanceState as backup to handle system-initiated process death. 

The onSavedInstanceState() callback stores data needed to reload the state of a UI controller such as an activity or a fragment, if the system destorys and later recreates that controller. 

Saved instance state bundles persist both configuration chagnes and process death, but are limited by storage and speed, becuase onSavedInstanceState() serializes data to disk. 
Serialization can consume a lot of memory if the objects being serialized are complicated. Becuase this process happens on the main thread during a configuration change, serialization can cuase dropped frames and visual stutter if it takes too long. 

Do not use store onSavedInstanceState() to store large amounts of data , such as bitmaps, nor complex data structures that require lengthy serialization or deserialization. Instead, store only primitive types and simple small objects such as String. As such, use onSavedInstanceState to store a minimal amount of data necessary, such as na ID, to re-crete the data necessary to restore the UI back to its previous state should the uother persistence mehcansisms fail. Most apps should impleemtn onSavedInstnaceState to handle system-initiated proccess death. 

Depending on your app's uses cases you  might not need to use onSavedInstate at all. For example a browser might take the user back to the exact webpage they were looking at before they exited the browser. If your activity behaves this way, you can fore go using onSaveInstnaceState() and instead persistet everyting locally. 

Additinoaly, when you open an activty from an intent, the bundle of extras is delivered to the activity both when the configuration changes and when the system restores the activity. If a piece of UI states data such as search query, were passed in as an intent extra when the activity was launched, you could use the extras bundle instead of the onSavedInstanceState() bundle. 

In either of these scenarios you should still use a ViewModel to avoid wasting cycles reloading data from the database during a configuration change. 

In cases where the UI data to preserve is simple and lightweight, you might use onSAveInstnaceState() alone to presever your state data. 
