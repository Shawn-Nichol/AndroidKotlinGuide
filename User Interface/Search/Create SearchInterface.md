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


## Perofrming search
Once you have declared your searchable activity in the manifest, performign a search in your searchable activity invovles three steps. 
1Receving the query
2. Searching your data
3. Presenting the results
Traditionally, your search results should presented in a ListView , so you might want your searchable activity to extedn ListActivity. It includes a default layout with a single listView and provides several convenience methods for working with the ListView. 

## Receving the query
When a user executes a search from the search dialog or widget, the system starts your searchable activity and sends it a `ACTION_SEARCH` intent. This intent carries the search query in the query in the Query string extra. You must check for this intent when the activity starts and extracting the string. For examplple, here's how you can get the search query when you searchable activity starts. 

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.search)

    // Verify the action and get the query
    if (Intent.ACTION_SEARCH == intent.action) {
        intent.getStringExtra(SearchManager.QUERY)?.also { query ->
            doMySearch(query)
        }
    }
}
```

The QUERY string is always included with the ACTION_SEARCH intent. In this example,the query is retrieved and passed to a local doMySerach() method where the actual search operation is deon. 

## Seraching  your data
The process of string and searching your data is unique to your application. You can store nad search your data in many ways, but this guide does not show you how to store your data and search it. Storing and searching your ddata is somehing you should carefully consider in terms of your needs and your data format. However, here are some tips you migh be able to apply

- If your data is stored in a SQLite database on the device, performing a full-text search (using FTS#, rather tha a like query can provide a more robust search across text data and can produce results significatnly faster. See sqlite.org for information about FTS3 and the sQLiteDatabase class for information about SQLite  on Andoird. 

- If youdata is stored online, then the perceived search perfromance might be inhibitied by the users' data connection. You migh want to display spinning progress wheel until your search returns.  See android for a reference of network APIs and creatinga progress dialog for infomration aobut how to disaply a progress wheel. 


Presenting the results Regardless of where your data lives an dhow you search it, we recommend that you return search results to your searchable activity witha n Aadapter. THis way, you caneasily present all the serach results in a ListView. If your data comes from a SQLite datase query , you can appply your results to a ListView using a CursorAdapter. if your data comes in some other type of format, then you can create an extension of baseAdapter. 

An adapter bind each items from a set of data into a view object. When the adapter is applied to a ListView, eaach piece of data is insertedas an individual view into the list. Adapter is just an interface, so implementations such aas CursorAdapter are needed. If none of the existing implementations work for your data, then you can implement your own from the BAseAdapter. 

You might want your searchable activity to extend ListActivity. Yolu can then call setListAdapter() passing it an Adapter that is bound to your data. THis injects all the search results into the activity ListVIew. 


## Using the serach dailog
THe serach dilaog provides a floating search box at the top of the screen, withthe application icon on the left. The search dialog can provide search suggestions as the user types and, when the user executes a serach, the styssjm sends the serach query to a searchable activity that perfroms the search. however, if you are developing your application for devices running Android 3.0, you should consider using the search widget instead. 

THe serach dialog is always hidden by defaul, until the user activiates it. Your application can icon on the left. THe search dialog can provide search suggestions as the user types and, when the user executes a search, he system sends thssearch query to sa searchable activity that perfroms the search. Howwever, if you are developing your application for devices running Android 3.0, you should consider using the serach widget instead. 

The search dialog is always hidden by default, until the user activates it. Your application can activate the search dialog by calling onSearchRequted(). However this method doesn't work until you enable the search dialog for the activity. 

To enable the serach dialog, you must indicate  to the system which searchable activity should receive search queries from the search dialog, in order to perform searches. For example, in the preevious section about Cretaing a SearchableActivity, a searchable activity named SearableAcitvity was created. If you want a seaparte activity, named OtherActivity, to show the search dialog and deliver seraches to searchaActivity, you must declare in the manifest that searchableActivity is the searable activity to use for the search dialgoo in OtherActivity. 

To declare the seracable activity for an activity's search dialog, add a <meta-data> elemetn insdie th respective activit's <activity> element. The '<meta-data>` element must includ eht andoird: value attribute that specifies the searchable activity's calas name and the android:name attribute with a value o android.app.default_searable"
  
For examplle, here is the declartion for both a searchable activity, sSEarchablActivity and another activity, other activity, whcih uses SearchablActivity to perfrom searches executed from its serach dialog. 

```
<application ... >
  // This is the searhable activity; it fperfrom searches 
  <activity android:name=".SearchableActivity">
    <intent-filter>
      <action android:name="android.intent.action.SEARCH"/>
    </intent-filter>
    <meta-data android:name="android.app.searchable"
          android:resource="@xml/searchable"/>
  </activity>
  
  // This activity enables the search dialog to initiate searches tin the SearchableActivity 
  <activity android:name=".otherActivity" ...>
    // enable the search dialog to send searches to SearchableActivity
    <meta-data android:name="android.app.default_searchable"
                android:value=".SearchableActivity" />
  </activity>
  
  ...
  
</application>
```

Becuase the OtherActivity now includes a <meta_data> element to declare which searchable activity to use for searches, the activity has enabled the search dialog. Hwile the user is in this activity, the onSearchREquested() method activates the search dialog. When the user executes the serach, the system starts SearchableActivity and delivers it the ACTION_SEARCH itent. 

If you want every activity in your applciation to provide the search dialog, insert the above <meta_data> element as a child of the <application> elemetn, instead of each <activity>. This way, every activity inherits the value, provides the seach dailog, and delivers searches to the same searchable activity.  If you ave multiple searchable activites, you can override the defaul tsearchable activity by placing a different <meta-data> declaration inside individual activites.
  
  With the search dialog now ineabled  fro your activites your application is ready to perfrom seraches
  
  ## Invoking the search dialog
  
  Althoug some  devices provide a dedicated Search button, the behavior of the button may vary between devices and many devices do not provide a  Serach button at all. So when using the search dialog, you must provide a search button in your UI that activates the serach dialog by callin g onSerachREquested()
  
  For instance, you should add a Search buttonn in your options menu or UI layout that calls onSearchREquested(). For consistency with the ndorid system and other apps, you should label your button with the Android Search icon that's avaiable from the Action bar Icon Pack. 
  
Note:  If you app uses an app bar then you should not use the search dialog for your search interface, instead, use the search widget as a collapsible view in the app bar. 

You can also enable d type-to-search functionality, which activatet the search dialog when the user starts typing on the keyboard- the keystorkes are inserted into the search dialog. You can enable type-to-search in your activity by callin gsetDefaultKeyMode(DEFAULT_KEYS_SERACH_LOCAL) during  your activity's onCreat() method

THe impact of the search dialog on your activity lifecycle. 


## The impact of the search dialog on your activty lifecycle
THe serch dialog is a Dialog that floats at the top of the screen. It down not cause any change in the activity stack, so when the search dialog appears, no lifecycle melthos (such as onPause()) are called. Your activity just loses input focus, as input focus is given to the search dailog. 

If you want to be notified when the serach dialog isa ctivated, override the onSearch rEquested() method. Whene the system calls this method, it is an indication that your activity has lost input focus to the search dialog, so you can do any work appropriate for the event (such as pause a game). Unless you are passing serach context data, you should end the method by calling the super class implementation

```
override fun onSearchRequested(): Boolean {
    pauseSomeStuff()
    return super.onSearchRequested()
}
```

If the user cancels serach by pressing the back button , the search dialog closes and the activity regains input focus. YOu can register to be notified when the serach dialog is closed iwth setONDimissListener() and or setOnCancelListener(). You should need to register only the OnDsmissListener, because it is called every tiem the serch dialog closes. The OnCancelListeren only pertains to events in twhich the users explicitly exited the serach dialog, so it is not calle dwhen a serach is executed(in which case, the search dialog natuarally disappears. 

If the current activity is not he serachable activit, then the normal activity lifecycle evetns are triggered once the user executes a s erach (the current activity recevies onPause() and so forth as described in the Activites docuemtn. If hosever, the current activit is the seraable activity, the one of two thing happens. 

1. By defaut, the searchable activity receives the ACTIONSERACH intent with a call to onCrearte() and a new insntance of the activity is brought to the top of the activity stack. There are now two instance of your searchable activity in the activity stack (so pressing the back button goes back to the previous instance  of the searchable activity, rather than exiting the serachable activity. 

2. If you set Android: laucnch Mode to singleTop then the searchabel activity receives the ACTION_SERACH intent with a call to onNewIntent(Intent), passing the news ACTION_SEARCH intent here. For example, here's how you might handle this case, in which the searchable activity's launch mode is singleTop

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.search)
    handleIntent(intent)
}

override fun onNewIntent(intent: Intent) {
    setIntent(intent)
    handleIntent(intent)
}

private fun handleIntent(intent: Intent) {
    if (Intent.ACTION_SEARCH == intent.action) {
        intent.getStringExtra(SearchManager.QUERY)?.also { query ->
            doMySearch(query)
        }
    }
}
```
Compared to the exmple code in the secontion about PEfroming a SEarch, all the code to handle the search intent i snow in the handleIntent() method, so that both onCreat() and onNewINtent() can execute it. 

When the system  calls onNewIntent(Intent0, the activity ahs not been restarted, so the getIntent() method returns the same intent that was received with onCreate() this is why you should call setInent(Intent) insdide onNewINtent(Intent) so that the intent saved by thea ctivity  is updated in case you call getInent in the future. 

The seocnd scenario using "singleTop" launch mode is uaully ideal, becuase chances are good that once a search is done, the user will perform additaionl searches and it's a bad experience if your application creates multiple instances of the serachable activity. So, we recommend that you set your searchable activity to "SintgleTop" launch mode in the applciation manifatest
```
<activity android:name=".SearchableActivity"
          android:launchMode="singleTop" >
    <intent-filter>
        <action android:name="android.intent.action.SEARCH" />
    </intent-filter>
    <meta-data android:name="android.app.searchable"
                      android:resource="@xml/searchable"/>
  </activity>
```

## Passing search context data
In some cases, you can make necessary refinements to the serach query inside the searchable activity, for every search made. However, if you want to refine  your search critereia based on the activity from which the user is perfroming a search, you can provide additional data in the intnet that the systme sends to your searchable activity. You can pass the additional datta in teh APP_DATA Bundel, which is included int eh ACTION_SERACH intent. 

To pass  this kind of data to your searhable activity, override the onSEarchrEquested() mehtod for the activit from which the user can perfrom a search, create a Bundle with the additionald data, and call startSearch(0 to actiavate the search dailog. 

```
override fun onSearchRequested(): Boolean {
    val appData = Bundle().apply {
        putBoolean(JARGON, true)
    }
    startSearch(null, false, appData, false)
    return true
}
```

Returing true indicate that you have  succesffully hadled this callback event and called starSearch() to activate the serahc dialog. Once the user submits a query, it's delivered to your serachable activity along with the data you've added. You can extract the eextra data from the  APP_DATA bundle to refine the serach 

```
val jargon: Boolean = intent.getBundleExtra(SearchManager.APP_DATA)?.getBoolean(JARGON) ?: false
```

## Using the serach widget 
The search view widget is aviable in android 3.0 and higher . If you're developing your application for android and have deciced to use the serach widget, we reocmmend that you insert the serach widget as an action view in the app bar, instead of using the serahc dialo (and instead of placing the serach widget in your activity layout f
The serch widget provides the same functionality as the serach dialog. It starts the appropriate activity when the user executes a search, and if can provide serach suggestion and perfrom voice serach. If it's not an  option for you to put the serach wigest in the ActionBar, you can instead bput the serch widget somewhere in your activiiyt layout. 

## Configuring the serach widget
After you've created a serachable configuration and a searchable activity, as discusssed above, you need to enable assisted serach for each SearchView. You can doso by calling setSEarchableINfo() and passing it the SearchableInfo object that represents your searchable setSErahcableInfo() and passing it the SearchableInfo object that represents your searchable configuration. 

You can get a referenc eto eh SearchableInfo by callin ggetSerachableINfo on SearchManager. 

For example if you're using a SeraachView as an action view in the app bar, you should enable the widget during the onCreatOptionsMenu() callback. 

```
override fun onCreateOptionsMenu(menu: Menu): Boolean {
    // Inflate the options menu from XML
    val inflater = menuInflater
    inflater.inflate(R.menu.options_menu, menu)

    // Get the SearchView and set the searchable configuration
    val searchManager = getSystemService(Context.SEARCH_SERVICE) as SearchManager
    (menu.findItem(R.id.menu_search).actionView as SearchView).apply {
        // Assumes current activity is the searchable activity
        setSearchableInfo(searchManager.getSearchableInfo(componentName))
        setIconifiedByDefault(false) // Do not iconify the widget; expand it by default
    }

    return true
}

```

That's all you need. the serach widget is now configured and the systme will deliver search queries to your serachable activity. You can also enable serach suggestion for the search widget 


## Other search widget features
A submit Button
By default here's no button to submit a asearch query, so the user must press the return key ont he keyboard o initiate a serach. You can add a submit button by calling setSubmitButtonEnbaled(true)

Query refinement for search suggestions
When you've enabled search suggestions, you usally expect users to simply select a suggestion, but they migh also want to refine the suggested search query. you can add a button alongside each suggestion that inserts the suggestion in the search box for refinement by the user, by callin g setQueryRefinementEnabled(true0

The ability to toggle the serach box visibility 
By  defaul the serach widget is iconfieied, meaning that it is represented only by a search icon an dexpands to show the search box when the user touches it. As shown above, you can show he search box by default, by calling setIconifedByDefault(false 
