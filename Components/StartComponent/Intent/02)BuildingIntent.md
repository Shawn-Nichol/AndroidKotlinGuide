# Building an Intent
An intent object carries information that the Android system usees to determine which components to start pluss information that the recipient component uses in order to prpoerly perform the action. The following properties represent the defining characteristics of an intent. By reading these properties, the android systme is able to resolve which app component it should start. However an intent can carry additional information that does not affect how it resolved to an app copmonent. 

- Componet name: The name of the componet to start. This is optional, but it's critical piece of information that makes an intent expliciit. Without a component name the Intent is impicit and the system decides which component should receive the intent based on the other intent information
- Action: A string that specifies the generic action to perform. In the case of a broadcastintent, this is the action that took place and is being reproted. The action largely determines how the rest of the intent is structured particularly the information that is contained in the data and extras

You can specify your own actions for use by intents within your app, but you usually specify action constants defined by th e intent class or other framework classes. 

ex ACTION_VIEW use this action in an intent iwth startActivity() when you have some information that an activity ccan show to the user, such as a photo to view in  a gallery app, or an address to view in a map app. 

ex ACTION_SEND, Also known as the share intent, you should use this in an intent with startActivity() when you have some datat that the user can share through another app, such as an email app or social sharing app. 

You can specify the action for an intent with setAction() or with an Intent constructor. 

- Data the URI that references the data to be acted on anand or the MIME type of that data. The type of data supplied is generally dicated by the intent's action.


When creating an intent, it's often important to specify the type of data in addition to its URI. For example an activity that's able to display images probably  won't be able to play an audio file, even though the URI formats could be similar. Specifiying the MIME type of your data helps the Android systme find the best component to reeive your intent. However the MIME type can sometimes be inferred from the URI particularly when the data i sa content: URI. A content URI indicates the data is located on the device and controlled by a ContentProvider wihc makes the data MIME type visible to the system. 

- Category  A string containing additiona l information about the kind of component that should handlle the  intent. Any number of category descriptions can be placed in an intent, but most intents do not require a category

ex CATEGORY_BROWSABLE The target activity a llows itself to be started by a web browser to display data referenced by a link such as an image or an email

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


