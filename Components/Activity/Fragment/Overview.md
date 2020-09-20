









## Creating event callbacks to the activity
In some cases, you might need a fragment to share events or data with the activity and or the other fragments hosted by the activity. To share data, create a shared ViewModel, as outolined in the share data between  fragments section in the ViewModel guide. If you need to propagate events that cannot be handled with a ViewModel, you can instead define a clallback interface inside the fragment and require that the host activity implement it. When the activity receives a callback throughtt the interface, it can share the information with other fragments in the layout as necessary. 

For example, if a news application has two fragments in an activity one to show a list of articles (fragment A) and another to display an article (fragment B) then fragment A must tell the activity when a list item is selected so that it can tell fragment B to display the article. In this case, the OnArticleSelectedListener interface is declared inside fragment A

```
public class FragmentA : ListFragment() {
  ...
  container Activity must impleemnt this interface 
  intervace OnArticleSelectedListener {
    fun onArticleSelected(articleUri: Uri)
  }
  ...
}
```

Then the activity that hosts the fragment implements the OnArticleSelectedListener interface and overrides onArticleSeelcted() to notify fragment B of the event from fragment A. To ensure that the host activity implements this interface, fragment A's on onAttach() callback method (which the system calls when adding the gramgent to the activity) instantiates an instance of OnArticleSelectedListener by casting the Activity that is passed into onAttach(). 
```
public class FragmentA : ListFragment() {
  var listener: OnArticleSelectedListener? = null
  ...
  override fun onAttach(context: Context) {
    super.onAttach(context0
    listener = context as? OnArticleSelectedListener
    if(listener == null) {
      throw ClassCastException("$context must implement onArticleSeelctedListed")
    }
  }
  ...
}
```

If the activity hasn't implemented the interface, then the fragment throws a ClassCastEception . On success, the mListener member holds a reference to activity's implementation of OnARticleSelectedListener, so that fragment A can share events with teh activity by calling methods defined by teh onARticleSelectedListener interface. For example, if fragment A is an extension of ListFragment, each time the user clicks a list item, the system calls onListItemClick() in the fragment, which then calls onArticleSelected9) to share the event with the activity:
```
public class FragmentA : ListFragment() {
  var listener: OnARticleSelectedListener? = null
  ...
  override fun onListeItemClick(l: ListView, v: View, position: Int, id: Long) {
    // append the click item's ro ID with the content provider Uri
    val noteUri: Uri = ContentUris.withAppendedID(ArticleColumns.CONTENT_URI, id)
    // send the event and th Uri to the host activity
    listener?.onArticleSelected(noteUri)
  }
}
```

The id parameter passed onListItemClick() is the row ID of the clicked item, which the activity(or other fragment) uses to fetch the article from the applications ContentProvider.




Once the activity reaches the resumed state, you can feely add and remove fragments to the activity. Thus, only while the activity is in the resumed state can the lifecycle of a fragment change independently. 

However, whenthe activity leaves the resumed state, the fragment again is pushed through its lifecycle by the activity. 

To create a fragment you must create a subclass of fragment. The fragment class has code that looks like an activity. it contains callback methos similar to an activity, such as onCreate(), onStart(), onPause(). In fact, if you're converting an existing Android application to use fragments you might simply move the code from your activity's callback methods into the respective methods in the fragment. 
