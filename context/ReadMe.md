# Context

Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activites, broadcasting and receivign intents. 

A context provides access to information about the application state. It provides Activites, Fragments, and Services access to resource files, images, themes/styles and external directory locations. It also enables access to androids built-in services such as those used for layout inflation, keyboard, and finding content providers. 

In many cases when the context is required, we simply need to pass in the instance of teh curretn activity. In situations where we are inside objects created by teh activity such as adapters or fragments,we need to pass tin the activity instance into those objects. In situations where we are outside of an activity, we can use application context instead. 
