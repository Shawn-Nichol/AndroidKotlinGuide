# Intent
An intent is an abstract desciption of an operation to be performed. It can be used with Context@startActivity(Intent) to launch an Activity, broadcastIntent to send it to any interested BroadcastReceiver components, and Context. startService(Intent) or Context.bindService(Intent, ServiceConnection, int) to communicate with a background Service. 

An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activites, where it can be thought of as the glue between activites. It is basicallly a passive data structure holding an absstract description of an action to be performed. 

## Intent Structure
The primary pieces of information in an intent are
- action: The general action to be performed, such as ACTION_VIEW, ACTION_EDIT, ACTION_MAIN
- data: The data to operate on, such as a person record in teh contacts database, expressed as a Uri. 

## Intent Resolution
There are two primary forms of intents
- Explicit Intents: Have specified a component (via setComponent(ComponentName) or setClass(Context, class)), which providees the exact class to be run. Often these will not include any other information, simply being a way for an application to launch various internal activites it has as the user interacts with teh application.
- Implicit Intents: Have not specified a component; instead they must includ enough information for the system to deteremine which of the available components is best to run for that intent. 

When using implicit intents given such an arbitrary intent we need to know what to do with it. This is hadled by the process of Intent resolution, which maps an Intent to an Activity, BroadcastReceiver, or Service(or sometimes two or more activites/ receivers that can handle it. 

The Intent resollution mechanism basically resvolves around matching an Intent against all of the <intent-filter> descriptions in teh installed application packages. Plus , in the case of broadcasts, any BroadcastReceiver objects explicityly registered with Context#registerReceiver. 

There are three pieces of information in the intent that are used for resolution: the action, type and category. Using this information, a query is done on teh PackageManager for a component that can handle the intent. The appropriate component is determined based on teh intent information supplied in teh AndroidManifest.xml file as follows:
- The action, if given, must be listed by the component as one it handles
- The type is retrieved fromt the Intent's data, if not already supplied in the intent. Like the action, if a type is included in the intent(either explicityly or implicitly in its data), then this must be listed by the component as one it handles. 
- For data that is not a content: URI and where no explicit type is included in the Intent, instead the scheme of the intent data (such as http: or  mailto:) is considrered. Again like the action if we are matching a scheme it must be listed by the conponent as one it can handle. 
- The categories, if supplied, must all be  listed by the activity as categories it handles. That is, if you include the categories CATEGORY_LAUNCHER and CATEGORY_ALTERNATIVE,  then you will only resolve to components with an intent that lists both of those categories. Activites will very often need to suppport the CATEGORY_DEFAULT so that they can be found by Context#startActivity

For example consider the Note pad sample application that allows a user to browse through a list of notes data and view details about individaul items. Text in italics indicates places where you would replace a name with one specific to your won package. 
