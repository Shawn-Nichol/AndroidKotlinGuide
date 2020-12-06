# Create a Search Interface
When you're ready to add search functionality to your aplication, Android helps you implement the user interface with either a search dialog that appears at the top of the activity windows or a serarch widget that you can insert in your layout. Both the search dialog and the widget can deliver the user's search query to a specific activity inyour application. This way, the user can initiate a search from any activity where the search dialog or widget is avaiablable, and the system sstarts the appropriate activity to perfrom the search and prseent results. 

## Basics
Search dialog is a UI componbet that's controlled by the Anroid system. When activated by the user, the search diloag appears at the top of the activity. 

The android system controls  all events in the search dialog. When the user submits a query, the system delivers the query to athe activity that you specify to handle searches. The dialog can also provide search suggestions while the user types. 

- The search widget is an instance of SearchView that you can place anywhere in your layout. By default, the search widget behaves like a standar EditText Widget and doesn't do anything, but you can configure it so that the Android system handles all input events, delivers queries to the appropriate activity, and provides earch suggestions. 

When the user executes a search from the search dialog or a search widget, the system createsan INtent and stores the user query in it. THe ssytem then starts the activity that you've declared to handle searches and delivers it the intent. To set  up your application from this kind of assited search oyu need thte following. 

- A searchable configuration
An XML file that confiures some setting sfromt he search dialog or widget. It includes settings for features such as voice, suggestions, and hint text for the search box. 
- A searchable activity
The activity that receives the search query , searches your data and displays the search results
- A search interface, 
  - The search dialog
  By default, the search dialog is hidden, but apperas at the top of the screen when you call onSearchREquesteed()
  - Or a Search view Widget 
  Using the search widget allows you to put the search box anywhere in your activity. Instead of putting it in your activity layout, you should usually use search View as an action view in teh app bar. 
  
 ## Creating a searchable configuration 
The first thing you need is an XML file called the searchable configuration. If conifgures certain UI aspects of the search dialog or widget and defines how features succh as suggestion and voice search behave. This file is traditionally named searchable.xml and must be saved in teh res/xml

The searchable configration file must include the <searchable> element as the root node and specifyg one of more attributes. 
```
 <?xml version="1.0" encoding="utf-8"?>
<searchable xmlns:android="http://schemas.android.com/apk/res/android"
    android:label="@string/app_label"
    android:hint="@string/search_hint" >
</searchable> 
```
  
The android:lable attribute is the only required attribute. It points to a string resource, which should be the application name. THis label isn't actually visible to the user until you enable search suggestions for Quick sesarch box. At that point, this label is  visible in the list of Searchabl eitems in the system settings. 

Trough it's not required, we recommend that you always include the android:hint attribute, which provides a hint strin gin teh search box before users enter a query. The hint is important beuase it provides important clues to users about what they can search. 

The `<searchable>` element acepts several other attributes. however, you don't need most attributes until you add features such as search suggestions and voice search. For detailed information about the searchabl econfiguration file, 

## Create a SEarchable Activity
A searchable activity is the activity inyour application that perfroms searches based on a query string an dpresents the search results. 

When the user executes a search in teh serach dialog or widget, the system starts your searchable activity and delivers it the search query in an INtent with the ACTION_SEARCH action. Your searchable activity retrieves the query fromt he intent's QUERY extra, then seraches your data na dpresents the results. 

Because you may include the search dialog or widget in any other activity in your application, the system ust known which activity is your searchabl eactivity, so it can properly deliver the search query. So you must first declare your searchable activity inthe Android manifest file. 

## Declaring a searchable activity
If you don't have one already, create an Activity that will perofrm searches an prsent results. you don't need to implement he serach functionality yet just create an activity that you can declare in the manifest. Inside the manifest `<activity>` element. 
1. Delcare the activity to accep tht e ACTION_SEARCH  intent, in an <intent-filter> element. 
2. Specify the searchable configuration to use, in a `<meta_data>` element. 
```
  <application ... >
    <activity android:name=".SearchableActivity" >
        <intent-filter>
            <action android:name="android.intent.action.SEARCH" />
        </intent-filter>
        <meta-data android:name="android.app.searchable"
                   android:resource="@xml/searchable"/>
    </activity>
    ...
</application>
```
  
The `<meta-data>` elment must include the `android:name` attribute with a value of android.app.searchable: and the android: resource attribute with a reference ot the searchabl econfigration file

