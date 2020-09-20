# Build a flexible UI
When designing your application to support a wide range of screen sizes, you can reuse your fragments in different layout configurations to optimize the user experience based on the available screen space. 

For example, on a handset device it might be appropriate to display just one fragment at a time for simgle-pane user interface. Conversely, you may want to set fragments side-by-side on a table which hasa wider screen sizez to display more information to the user. 

The Fragmentmanager class provides methods that allow you to add, remove and replace fragments to an activity at runtime in order to create a dynamic experience. 

## Add a Fragment to Activity Runtime
To perfrom a transaction such as add or remove a fragment, you must use the FragmentManager to create a FragmentTransaction, which provides APIs to add, remove replace, and perfrom other fragment transactions. 

If your activity allows the fragment to be removed and replaed, you should add the initial fragment(s) to the activity during the activity's onCreate() method


```
res/layout/news_articles.xml

<FrameLayout slmns: android='http://schema.android.com/apk/res/android"
  android: id = "@id/fragmentcontainer"
  android:layout_with="matchparent"/>
 
```

Inside your activity calls getSupportManager to add a FragmentManger using the support Library API's then call begin Transaction () to create. Then call beginTransaction for the activity using the same FragmentTransaction. When you're ready to make the changes, you must call commit()

```
class MainActivity : FragmentActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.news_articles
    
    // Check that the activity is using the layout version with the fragment_container FrameLayout.
    if(findViewById(R.id.fragment_container) != null) {
      if(savedInstanceState != null) {
        return
      }
      
      supportFragmentManager.commit {
        // Create a new Fragment to be placed in the activity alyout using. Add the frament to the fragment_container FrameLayout.
        // In case this activity was started with special instructions from 
      }
    }
  }
}
```

Becuase the fragment has been added to the FrameLayout contianer at runtime instead of defining it in the activitys layout with a <fragment element-the activity can remove the fragment and replace it with a different one. 

# Replace One Fragment with another
The procedure to replace a fragment is similar to adding one, but requires the replace() method instead of add().

Keep in mind that when you perfrom fragment transactions, such as replace or remove one, its often appropriate to allow the user to navigate backward and "undo" the change. To allow the user to navigate backward throught the fragment transactions, you must call addToBackStack() before you commit the FragmentTransaction. 

Note: When you remove or replace a fragment and add the transaction ot the back stack, the fragment that is removed is stopped (not destoryed). If the user navigates back to restore the fragment, it restarts. If you do not add the transaction to the back stack, then the fragment i destoyed when removed or replaced. 
