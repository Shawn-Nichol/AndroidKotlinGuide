# Configuration changes
If the configuration of the device changes then anything displaying a user interface will need to be update to match that configuration. Because Activity is the primary mechanism for interacting with the user, it includes special support for handling configuration changes.

Unless you specify otherwise a configuration change will cause your current activity to be destroyed, going through the normal activity lifecycle process of onPause(), onStop(), and onDestroy() as appropriate. If the activity had been in the foreground or visible to the user, once onDestroy() is called in that instance then a new instnace of the activity will be created, with whatever savedInstanceState the previous instnace had generated from onSaveInstanceState(bundle).

This is done becuase any application resource, including layout files, can change based on any configuration value. Thus the only safe way to handle a configuration change is to re-retrieve all resouces, including layouts drawables, and strings. Becuase activites must already know how to save their state and re-create themeselves from that stae, this is a convenient way to have an activity restart itself with a new configuration.

In some special cases, you may want to bypass restarting of your activity based on one or more types of configuration changes. This is done with the android:configChanges attribute in the manifest. For anytypes of configuration changes you say that you handle there, you will receive a call to your current activity's method instead of being restarted. If a configuration change involves any that you do not handle, however the activity will still be restarted and onConfigurationChange will not be called.

## Landscape XML
Landscape XML are stored in the layout-land directory. You can create a landscape xml file by click on the orientation preview tab and selecting create landscape orientation. 


