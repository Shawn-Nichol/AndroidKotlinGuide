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
