# Building an Intent
An intent object carries information that the Android system usees to determine which components to start pluss information that the recipient component uses in order to properly perform the action. By reading these properties, the android systme is able to resolve which app component it should start. However an intent can carry additional information that does not affect how it resolved to an app copmonent. 



## Explicit Intent
An explict event requires context and componet name, (without a component name it is implicit intent). This allows you to launch a specific component. 

MainActivity
```
// Create a handler, 
val intent: Intent = INtent(this, SecondActivity::class.java).apply {
    putExtra("MY_DATA_KEY", "My string that I want to pass")
}

// Verify that the intent will resolve to an activity
if(intent.resoleActivity(packagenManager) != null) {
    // Launches a new activity.
    startActivity(intent)
}
```
To receive the data in the second activity
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_two)
    
    // Create a handler for the Bundle
    val extra: Bundle? = intent.extras
    
    // Use the key in the extra handler to get the data you want. 
    val text: String? = extras?.getString("MY_DATA_KEY")
}
```


## Imiplict Intent
Specifies an action that can invokek any app on the device able to perform the action. Using an implicit intent when your app cannot perform the action but other apps can. 

ex ACTION_VIEW use this action in an intent iwth startActivity() when you have some information that an activity ccan show to the user, such as a photo to view in  a gallery app, or an address to view in a map app. 

ex ACTION_SEND, Also known as the share intent, you should use this in an intent with startActivity() when you have some datat that the user can share through another app, such as an email app or social sharing app. 

You can specify the action for an intent with setAction() or with an Intent constructor. 

- Data the URI that references the data to be acted on and or the MIME type of that data. The type of data supplied is generally dicated by the intent's action.

When creating an intent, it's often important to specify the type of data in addition to its URI. For example an activity that's able to display images probably won't be able to play an audio file, even though the URI formats could be similar. Specifiying the MIME type of your data helps the Android system find the best component to revive your intent. However the MIME type can sometimes be inferred from the URI particularly when the data is a content: URI. A content URI indicates the data is located on the device and controlled by a ContentProvider which makes the data MIME type visible to the system. 

- Category  A string containing additional information about the kind of component that should handle the intent. Any number of category descriptions can be placed in an intent, but most intents do not require a category

ex CATEGORY_BROWSABLE The target activity allows itself to be started by a web browser to display data referenced by a link such as an image or an email

ex CATEGORY_LAUNCHER The activity is the initial activity of a task and is listed in the system's application launcher, you can specify category with addCategory()

- Extra Key-value pairs that carry additional information requrired to acomplish the requested action. Just as some actions use particular kinds of data URIs, some action also use partcular extras. You can add extra data with various putExtra() methods, each accepting two parameters the key name and the value. You can also create a Bundle object with all the extrra data, then insert the Bundle in the intent with putExtra().

For example when createing an intent to send an email with ACTION_SEND, you specify to the recipient with the EXTRA_EMAIL key, and specify the subject with the EXTRA_SUBJECT key. 

For example when creating an intent to send an email with ACTION_SEND, you specify to the recipient with the EXTRA_EMAIL key, and specify the subject with the EXTRA_SUBJECT key. 

The intent class specifies many EXTRA_* constants for standardized data types. If you need to declare your own extra keys, be sure to include your app's package name as a prefix. 

- Flags are defined in the intent class that function as metadata for the intent. The flags may instruct the Android system how to launch an activity and how to treat it after it's launched. 


Implicit intent
```
// Create a text message with a string
val sendIntent = Intent().apply}
    action = Intent.ACTION_SEND
    putExtra(Intent.EXTRA_TEXT, textMessage)
    type = "text/plan"
}

// Verify that the intent will resolve to an activity
if(sendIntent.resolveActivity(packageManager) != null) {
    startActivity(sendIntent)
}

// When startActivity() is called, the system examines all the installed apps to determine which one can handle this kind of intent. If multiple activites accept the intent,
// the system displays a dialog asking you to select an app. 
```


