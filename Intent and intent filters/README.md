# What are Intents and intent filters
An Intent is a messaging object you can use to request an action from another app component. Altough intents facilitate communication between componetns in several ways, there are three fundamental use cases.

1) Starting an Activity
The Intent describes the activity to start and carries any necesssary data. 

2) Starting a service
The Intent descibes the Service and carries any necessary data. 

3) Delivering a broadcast

## There are two types of intents.

1) Explicit
Specifies which application will satisfy the intent, by supplying either the target app's package name or a fully-qualified component class name. You'll typically use an explicit intinet to start a component in oyour own app, becuase you know the class name of the acitivty or service you want to start. 

2) Implicit 
Do not name a specific component, but instead declare a general action to perform, which allows a component from another app to handle it.

When you use an implicit intnet, the Android system finds the appropriate componet to start by comparing the contents of the intent to the intent filters declared in the manifest file or other appss on the device. If the intent  matches an intent filter, the systme starts that component and delivers it the Intent object.  If multiple intents filters are compatible, the systme displays a dialog so the user can pick which app to use. 

An intent filter is an expression in an app's manifest file that specifies the type of intents that the componet would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise, if you do not declare any intent filters for an activity, then it can be started only with an explicit intent. 


## Building An Intent
An Intent object carries information that the android systme uses to determine which component to start(such as the exact component name or component category that should receive the intent), plus information that the recipient componet uses in order to properly perform the action(such as the action to take and the data to act upon). 

## Component name:
The name of the component to start. This is optional, but it's the critical piece of information that makes an intnet explicit, meaning that the intent should be delivered only to the app conponet defined by the component name. Without a component name, the intent is implicit and the systme decides which component should receive the intnet based on the other intent information(such as the action, data, and category described below). If you need to sttart a specific component in your app, you should specify the component name. 

The field of the Intent is a ComponentName object, which you cna specify using a fully qualified class name of the target component, including the package name of the app, for . Youcan set he component name with `setComponent()`, `setClass()`, `setClassName()`, or with the Intent constructor. 

## Action
A stirng that specifies the generic action to perform (such as view or pick)

In the case of a broadcast intent, this is the action that took place and is being reported. Th eactino largely determines how the rest of the intent is structured-particularly the informatio nthat is contained in teh data and extras. 

you can specify your own actions for use by intents within your app (or for use by other apps to invoke components in your app), but you usually specify action constants defined by the Intent class or other framework classes. here are some common actions for startintg an activity

`ACTION_VIEW`
Use this action in an intent with `startActivity()` when you have some infomration that an activity can show to the user, such as a photo to view in a gallery app, or an address to view in a map app. 

`ACTION_SEND`
Also known as the share intent, you should use this in an intent with `startActivity()` when you ahve some data that the user can share through another app, such as an email app or social sharing app. 

Specify the actionfor an intent with `setAction()` or with an Inent constructor.

## Data
The URI that references the data to be acted on and or the MIME type of that data. The type of data supplied is generally dicated by the intents actionn. For example, if the actin is `ACTION_EDIT`, the data should contain the URI of the document to edit. 

When creating an intent, it's often important to specify the type of data in addition to its URI. For example, an activity that's able to display images probably won't be able to play an audio file, even though the URI formats could be simlar. Specifying the MIME type of your data helps the Android systme find the best component to receive your intent. However, the MIME type can sometimes be inferred fromt eh URI -praticualarly when the data is a content: URI. A content: URI indicates the dat is located on the device and controlled by a ContentProvider, which makes the dataMIME type visible to the system. 

To set only the dat URI, call setData(). Tos et only the MIME type, call setType(). IF necessary, you can set both explicitly with setDataAndType()


## Category
A string containt addtional information about the kind of compoonent that should handle the intent. Any number of cateogyr descitpions can be placed in an intent, but most inents do not require a category. 

`CATEGORY_BROWSABLE`
The target activity allows itself to be started by a web browser to display data referenced by a link, such as an image or an e-mail message.

`CATEGORY_LAUNCHER`
The activity is the intial activity of a task and is listed in teh system's application launcher. 

These properties listed above represent the defining characterisics of an intent. By reading these properties, the Android system is able to resolve which app component it should start. However, an intent can carry additional information that does not affect how it is resolved to an app component. An intent cna also suppply the following infomration

## Extras
key-value pairs that carry additional information required to accomplish the requested action. Jsut as some actions use particular kinds of data URIs, some actions also use partiuclar extras.
You can add extra data with various `putExtra()` methos, each accepting two parameters: the key name and the value. you can also create a `Bundle` object with all the extra data, then insert the Bundle in the Inent with putExtras(). 

For example, when creating an intent to send an email with `ACTION_SEND`, you can specify the to recipient with the EXTRA_EMAIL key, and specify the subject with the EXTRA_SUBJECT key. 

The intent class specifies many `EXTRA*` constants for standardized data types. If you need to declare your own extra keys (for intents that your app recieves), be sure to include your app's package name as a prefix. 
```
const val EXTRA_GIGWATTS = "com.exmaple.EXTRA_GIGAWATTS"
```

### Flags
Flags are dfined in the Inent class that function as metadata for th eintent. The flags may instruct the Android systme how to launch an activity and how to treat it after it's launched. 

## Example explicit inent
An explicit intent is on ethat you use to launch a specific app component, sucha ss a particular activity or service in your app. To create an explicit intent, define the component name fot he Intent object-all other intent proeprtis are optional. 
```
val downloadItnet = Intent(this, DownloadService::class.java).apply {
  data = Uri.parse(fileUrl)
}
startService(downloadIntent)
```

The Intent(Context, class) constructor supplies the app Context and the component a Class object. As such this inetn explicityly starts the DowloadService class in teh app. 

## Example implicit inent. 
An implicit inent specifies an action that can invoke any app on the device able to perform the action. using an implicit intent is useful when your app cannot perform the action, but other apps probalbly can and you'ld liek the user to pick which app to use. 

For example, if you ahve conten tthat you want the user to share with other people, create an innent with the `ACTION_SEND` and add extras that specify the content to share. Wehn you call `startActivity()` with that intent, the user can pick an app through wich to share the content. 

```
// Create the text message witha  string
val sendIntent = Intent().apply {
  action = Intent.ACTION_SEND
  putExtra(Intent.EXTRA_TEXT, textMessage)
  type = "text/plain"
}

// Verify that the intent will resolve to an activity
if(sendIntent.resolveActivit(packageManager) != null) {
  startActivity(sendIntent)
}
```

When `startActivity()` is called, the system examines all of the installed apps to determine which ones can handle this kind of intent (na intent with the `ACTION_SEND` action that carries "text/plain" data). IF there's only one app that can handle it, that app opens immediatlely and is given the intent. If multiple apps can handl a dailog window will open allowing the user to choose the app. 

### Forcing app Chooser
When there is more than one app that respons to you r implicit intent, the user can selectw hich app to use and make that app the default choice fot he action. The ability to select a default is helpful when performing an action for which the user probably wants to use the same app everytime, such as when opening a webpage. 

However, if multiple apps can respond to the intent and the user might want to use a different app each time, you should explicitly show a chooser dialog. The chooser dialgo asks the suer to select which app to use for the actio. 

To show the chooser, create an Intent using `createChooser()` and pass it to `startActivity()`
```
val sendIntent = Intent(Intent.ACTION_SEND)
...

val title: String = "MyChooser"
val chooser: Itnent = Intent.createChooser(sendIntent, title)

// Verifry the oringal inent will resolve to at least one activity
if(sendInent.resolveActivity(packageManager) != null) {
  startActivity()
}
```

## Receiving an implicit intent. 
