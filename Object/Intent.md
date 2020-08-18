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


## Intents and Intent Filters
An Intent is a messaging object you can use to request an action from another app component. Although intent facilitate communication between components in several ways, there are three fundamental use cases
- Starting an Activity
An Acitvity represents a single screen in an app. You can start a new instance of an Activity by passing an Intent to startActivity(). The Intent descibes the activity to start and carries any necessary data. 

If you want to receive a result from the activity when it finishes, call startActivityForResult(). Your activity receives the result as a separate Intent object in your activit's onActivitResult() callback. For more information, see the Activites guide. 
- Starting a service
A Service is component that performs operations in the background without a user interface, the intent descibes the service to start and carries any necessary data. 

If the service is designed with a client-server interface, you can bind to the service from another conponetn by passing an Intent to bindSErvice().

- Delivering a broadcast
A broadcast is a message taht any app can receive. The system delivers various broadcasts for system evetns, such as when the system boots up or the device starts charging. You can deliver a broadcast ot other apps by passing an Intent to sendBroadcast() or asendOrderedBroadcast()


## Types of Intents
- Explicit intents specify which application willl satisfy the intent, by supplying either the target app's package name or a fully-qualifited conponent class name. Youo'll typically use an explicit intent to start a conponent in your own app, becuase you know the class name of the activity or service you want to start. For example you might start a new activity within your app in response to a use action, or start a service to download a file in the background. 

- Implicit intent do not name a specific conponent, but instead declare a general action to perfrom, which allows a conoponent from another app to handle it. For example, if you want to show the usera  locationon a map, you can use an implicit intent to request that another capable app show a specified location on a map. 

When you use an implicit intent, the Android system finds the appropriate conponet to start by compaing the contents of the intent to the intent filters declared in teh mainfest file of other apps on the device. If th eintnet matches an intent filter, the sytme starts that conponent and elivers it the Intent object. If multiple itnetn filters are conpatible the sytsem displays a dialog so the user can pick which app to use. 

An intent filter is an expression in app's manifest file that specifies the type onintents that the onponent would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise if you so not delcare any intent filters for an activity then it can be started only with an explicit intent. 

To ensure that your app is secure, always use an explicit intent when starting a service and do not declare intent filters for your services. Using an implicit intent to start a service is a security hazard becuase you can't be certain what service will respoond to the intent, and the user can't see which service starts. 


## Building an intent
An intent object carries information that the Android systme uses to determine which conponent to start(such as the exact conponent name of conponent category that should receive the intent), plus information that the recipient conponent uses in order to properly perform the action (such as the afction to take and the data to act upon)

The primary information contained in an Intent is the following

### Conponetn name
The name of the conponent to start. THis is option, but it's the critical piece of information that makes an intent explicit, meaning that the intent should be delivered only to the app conponent defined by the conponent name. Without a conponent name, the intent is implicit and the system decides which ocnponet should recieve th eintent based on the other intent information(such as the action, data and category-described below). If ouyu need to start a specific conponent in your app, you should specify the component name. 

THis filed of the intent is a ConponentName object, which you can specify using a fully qualified class name of the target conponent, including the package name of the app, for example, com.exampleExampleActivity. You can set the oncponent anme with setConpontent(), setClass(), setClassName(), or with the Intent constructor. 

### Action
A string that specifies the generic action to perform. In the case of a broadcast intent, this is the action that took place and is being reported. The actionlargely dtermines how the rest of th eintent is structured-particularly the information that is contained in the data and extras. 

You specify your own actions for use by intents within your app (or for use by other apps to invoke conponetns in your app), but you usually specify action contstants defined by the Intent class or other framework classes. Here are some connnom actions for starting an activity. 

ACTION_VIEW
Use this action in an intent with startActivit() when you have some information that an activity can show to the user, such as a photo to view in a gallery app, or an address to view in a map app. 

ACTION_SEND
Also known as the sahre intent, you should use this in an intent with startActivit() when you have some  data that the user can share through another app, such as an email app or social sharring app. 

SeeIntent class reference for more constant that define generic actions. Other actions are defined elsewhere in the Android framework, such as in Settings for actions that open specific screens in the sytem's Settings app. 

You can specify the action for a an intent with setAction() or with an Intent constructor. 

If you define you own actionns, be sure to include your app's package name as a prefix, as shown in the following
