# Creating a search Interface

### Search Dialog: 
is a UI component that's controlled by the Android system. When activated by the user, the search dialog appears at the top of the activity. The Android system controls all events in the search dialog. When the users ubmits a query, the systme delivers the query to the activit that you specify to handle searches. The dialog can also provide search suggestion while the user types. 

### Search Widget
Is an instance of SearchView that you can place anywehre in your layout. By default  the search widget behaves like a standard EditText widget and doesn't  do anything, but you can configure it so that the Andrdoi ssytme handles all input events, delivers queries to the appropriate activity and prvodes search suggestions.  


When The user executes a  search form the search dialog or a search widget, the system creates an Intent and store the user query in i t. The system  then starts the activity that you've dclared to handle searches and dleivers it the intent. To setup your application for this kind of assicsted search, you need the following. 

- A searchable configuration 
An XML file that confiures some settings for the serach dialog or widget. it includes settings for features such as voice, search search suggestions, and hint text for the serach box. 

- A searchable activity
The activity that receives the search query, searches your data and displays the search restuls 

- A search interface, provided by either
  - Dialog: By default, the search dialog is hiden, but apperas at the top of the screen when you call onSearchRequested() 
  - SearchView: Using the search widget allows you to put the search box anywhere in your activity. 
  
  
## Creating a serach Configuration 
Create an XML file called the searchable configuration. It configres certain UI aspects of the search dialog or widget and defines how features such as suggestions and voice search behave. This file is traditionally named searchable.xml and must be saved in the res/xml directory. 

The searchable configuration file must cinclude the <searchable> element as the root node and specify oneor more attributes. 
```
<?xml version="1.0" encoding="utf-8"?>
<searchable xmlns:android="http://schemas.android.com/apk/res/android"
    android:label="@string/app_label"
    android:hint="@string/search_hint" >
</searchable>
```

The android label attribute is  the only required attribute. 


## Creating a Searchable activity
A searchable activity is the Activity inyour application that perfroms earches based on a query string and pressent the search results. 

When the user executes a search in the search dialog or widget, the systme starts your searchable activity and delivers it the serach query in an Intent with the ACTION_SEARCH. Your searchable activity retrieves the query from the intents QUER extra, then seraches your data and presents the results. 

Because you may include the search dialog or widget in  any other activity in your application, the system must know which activity is your searchable activity, so it cna properly deliver the search query. So, you must first declare your seachable activity in the Android manifest file. 


## Declaring a searchable activity 
If you don't have one already, create an Activity that will perofrom seraches an preseent results. You don't need to implement the search functionality yet just create an activity that you can declare in the manifest. Inside the manifests activity element. 

1. Declar ehte activity tto accept hte ACTION_SERACH  intent, in an <intent-filter> element 
2. Specify the serachabl confiration ito use, in a <meta-data> element
  
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

The <meta-data> element mus include the andorid:name attribute witha value of and resource attribute with a reference  othe searchabl confiration file
