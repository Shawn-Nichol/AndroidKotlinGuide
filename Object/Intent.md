# Intent
An Intent is a messaging object you can use to request an action from another app component. Although intents facilitate communication between conponents in several ways. there are three fundamental use cases

- Starting an Activity
You Can start a new instance of an activity by passing an Intent to startActivity(). The Intent descibes the activity to start and carries any necessary data. 

If you want to receive a result from an Activity when it finishes, call startActivityForResult(). Your activity receives the result as a separate Intent object in  your activit's onActivitResult() callback.

- Starting a service
You can start a service to perform a one-time operation (such as download song) by passing an Intent to startService(). The Intent describes the service to start and carries any necessary data. 

- Delvering a broadcast
The system delivers varous broadcasts for system events, such as when the systme boots up or the device starts charging. you can deliver a broadcast to other apps by passing an Intent to sendBroadcast() or sendOrderedBroadcast() 

An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activites, where it can be thought of as the glue between activites. It is basicallly a passive data structure holding an absstract description of an action to be performed. 

## Intent Structure
The primary pieces of information in an intent are
- action: The general action to be performed, such as ACTION_VIEW, ACTION_EDIT, ACTION_MAIN
- data: The data to operate on, such as a person record in teh contacts database, expressed as a Uri. 

## Types of Intents

- Explicit intents 
Specify which application will satisfy the intent, by supplying either the target app's package name or a fully-qualifited conponent class name. You'll typically use an explicit intent to start a conponent in your own app, because you know the class name of the activity or service you want to start. For example you might start a new activity within your app in response to a use action, or start a service to download a file in the background. 

- Implicit Intent
Do not name a specific conponent, but instead declare a general action to perfrom, which allows a components from another app to handle it. For example, if you want to show the user a location on a map, you can use an implicit intent to request that another capable app to show a specified location on a map. 

When you use an implicit intent, the Android system finds the appropriate conponet to start by compaing the contents of the intent to the intent filters declared in teh mainfest file of other apps on the device. If th eintnet matches an intent filter, the sytme starts that conponent and elivers it the Intent object. If multiple itnetn filters are conpatible the sytsem displays a dialog so the user can pick which app to use. 

An intent filter is an expression in app's manifest file that specifies the type onintents that the onponent would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise if you so not delcare any intent filters for an activity then it can be started only with an explicit intent. 


## Building an intent
An intent object carries information that the Android system uses to determine which conponent to start plus information that the recipient componet uses in order to properly perform the action. The followin properties represent the defining characteristics of an intent. By reading these properties, the Android system is able to resolve which app component it should start. However an intent can carry additional information that does no affect how it is resolved to an app componet. 

- Component name
The name of the componet to start. This is optional, but it's the critical piece of information that makes an intent explicit. Without a component name the Intent is implicit and the system decides which cmoponent should receive the intent based on the other intent information 

- Action 
A string that specifies the generic action to perform. In the case of a broadcast intent, this is the action that took place and is being reproted. The action largely determines how the rest of the intent is structured particularly the information that is contained in the data and extras. 

You can specify your own actions for use by intents within your app, but you usually specify action constants defined by the Intent class or other framework classes. 

ex ACTION_VIEW
Use this action in an intent with startActivity() when you have some information that an activity can show to the user, such as a photo to view in a gallery app, or an address to view in a map app. 

ex ACTION_SEND
Also known as the share intent, you should use this in an intent with startActivity() when you have some dta that the user can share through another app, such as an email app or social sharing app. 

You can specify the action fo an intent with setAction() or with an Intent constructor. 

- Data
The URI that references the data to be acted on and or the MIME type of that data. The type of data suppliedis generally dicated by the intent's action. 

When creating an intent, it's often important to specify the type of data in addition to its URI. For example an activity that's able to display images probably won't be able to play an audio file, even  though the URI formats could be similar. SPecifying the MIME type of your data helps the Android system find the best component to receive your intent. However the MIME type can sometimes be inferred from the URI  particularly when the data is a content: URI. A content: URI indicates the data is located on the device and controlled by a ContentPRovider, which makes the data MIME type visible to the system. 

- Cateogry
A string containg additional information about the kind of component that should handle the intent. Any number of category descriptions can be placed in an intent, but most intents do not require a category

ex CATEGORY_BROWSABLE
The target activity allows itself to be started by a web browser to display data referenced by a link such as an image or an e-mail message. 

ex CATEGORY_LAUNCHER
The activity is the initial activity of a task and is listed in the system's application launcher, you can specify category with addCategory()

- Extra
Key-value pairs that carry additional infomration required to accomplish the requested action. Just as some actions use particular kinds of data URIs, some actions also use particular extras.  You can add extra data with various putExtra() methods, each accepting two parameters the key name and the value. You can also create a Bundle object with all the extra data, then insert the Bundle in the Intent with putExtras(). 

For example when creating an intent to send an email with  ACTION_SEND, you specify the to recipient with the EXTRA_EMAIL key, and specify the subject with the EXTRA_SUBJECT key. 

The intent class specifies many EXTRA_* constants for standardized data types. If you need to declare your own extra keys, be sure to include your app's package name as a prefix.

- Flags
Flags are defined in the Intent class that function  as metadata for the intent. The flags may instruct the Android system how to launch an activity  and how to treat it after it's launched


Implicit Intnet
```
 Create the text message wit a string
 // Create the text message with a string. 
 val sendIntent = Intent().apply {
  action = Intent.ACTION_SEND
  putExtra(Intent.EXTRA_TEXT< textMessage)
  type = "Text/plain"
}

// Verify that the intent will resole to an activity
if(sendIntent.resolveActivity(packageManager) != null) {
  startActivity(sendIntent)
}
When startActivty() is called, the system examine all of the installed apps to determine which ones can handle this kind of intent. If there's only one app that can handle it, that app opens and is given the intent. If multiple activites accept the intent, the systme displays a dialog asking you to select an app. 


```

## Forcing an app chooser
When there is more than one app that responds to your implicit intent, the user can select which app to use an dmake that app the default choice for the action. The ability to select a default is helpful when performing an action for which the user probably wants to use the same app every time, such as when opening a web page. 

However, if multiple apps can respond to the intent and theuser might want to use a different app each timem, you should explicityly show a chooser dialog. The chooser dialog asks the user to select which app to use for the action. 

To show the chooser, create an intent using createChooser() and pass it to startActivity(). 
```
val sendIntent = INtent(Intent.ACTION_SEND)

// Always use string resource for UI text. 
// THis syas csomething like "Share this photo with"
val title: String = resource.getString(R.string.chooser_title)
// Create intent to show the chooser dialog
val chooser: Intent = Intent.createChooser(sendIntent, title)

// Verify the original intent will resolve to at least one activity
if(sendIntent.resolveActivity(packageManager) != null) {
  startActivity(chooser)
}
```

## Receiving an impicit intent
to advertise which impicit intents your app can receive, declare one or more intent filters for each of your app components with an <intent-filter> element in your manifest file. Each intent filter specifies the type of intents it accepts based on the intent's action, data and category. The system delivers an impicit intent to your app component only if the intent can pass through one of your intent filters. 
  
An app conponet should declare separate filters for each unique job it can do. For example, one activity in an image gallery app may have two filters: one filter to view an image and another filter to edit an image. When the activity starts, it inspects the Intent and decides how to behave based on the information in the Intent

Each intent filter is defined by an <intent-filter> element in the app's manifes file, nested in teh corresponding app conponent. Inside the <intent-filter> you can specify the type of intents to accept using one or more of thesee three elements, action, data, category. 
```
  <activity android:name="ShareActivity">
    <intent-filter>
       <action android: name="android.intent.action.SEND"/>
       <category android:name="android.intent.category.DEFAULT"/>
       <data android:mimeType="text/plain"/>
      </intent-filter>
  </activity>
```
You can create filter that includes more thanon instance of <action>, <data>, <category>. If you do, you need to be certain that the onponent can hnadle any and all combinations of those filter elements. 
  
When you want to handle multiple kinds of intents, but only in specific combinations of a ction, data and category type, then you need to create multiple intent filters. 

An implicit intent is tested against a filter by comparing the intent to each of the three elements. To be delivered to the conponet, the intent must pass all three tests. If it fails to match even one of them, the Android systme won't deliver the intent to the conponet. However, becuase  a conponet may have multiple intent filters, an inten that does not pass through one of a component's filterers might make it through on anoghter filter. 
```
<activity android:name="MainActivity">
    <!-- This activity is the main entry, should appear in app launcher -->
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity android:name="ShareActivity">
    <!-- This activity handles "SEND" actions with text data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
    <!-- This activity also handles "SEND" and "SEND_MULTIPLE" with media data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <action android:name="android.intent.action.SEND_MULTIPLE"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="application/vnd.google.panorama360+jpg"/>
        <data android:mimeType="image/*"/>
        <data android:mimeType="video/*"/>
    </intent-filter>
</activity>
```
The first activity, MainActivity, is the app's main entry point-the activity that opens when the user initially launches the app with the launcher icon
- The ACTION_MAIN action indicates this is the main entry point and does not expect any intent data. 
- The CATEGORY_LAUNCHER category indicates that this activity's icon should be placed in teh system's app launcher. If the <activity> element does not specify an icon with icon, then the system uses the icon from the <application> element. 
  
  These two must be paired together in order for the activity to appear in the app launcher. 
  
  The second activity, ShareActivity, is intended to facilitate sharing text and media content. Although users might enter this activity by navigating to it from MainActivity, they can also enter shareActivity directly from another app that issues an implicit intent matchign one of the two intent filters. 
