# Display paged liss
You can connect an instance of LiveDAta<PagedList> to a pagedListAdapter, as shown i the following code. 

```
class ConcertActivity : AppCompatActivity() {
  private val adapter = ConcertAdapter()
  
  // Use the by viewModels() kotlin property delegate
  // from teh activity-ktx artifact
  private val viewModel: ConcertViewModel by viewModels()
  
  override fun onCreate(savedInstanceState: Bundel? ) {
    viewModel.concerts.observe(this, Observer {adapter.submitList(it) })
  }
}

```

As data sources provide new instance of PagedList, the activity sends these objects to the adapter. The pagedListAdapter implementation defines how updates are computed, and it automatically handles paging and list diffinfg. Theere fore, your ViewHolder only needs to bind to a particular provided item.  
```
class ConcertAdapter() :
        PagedListAdapter<Concert, ConcertViewHolder>(DIFF_CALLBACK) {
    override fun onBindViewHolder(holder: ConcertViewHolder, position: Int) {
        val concert: Concert? = getItem(position)

        // Note that "concert" is a placeholder if it's null.
        holder.bindTo(concert)
    }

    companion object {
        private val DIFF_CALLBACK = ... // See Implement the diffing callback section.
    }
}

```
The PageListeAdapter handles page load events using a PagedList.Callback object. As the user scrollls, the PagedListAdapter calls PagedList.loadAround() to proide hints to the underlying PagedList as to which items it should fetch from the DataSource. 


## Implement the diffing callback
The folllwoing sample shows a manual implementation of areContentsTheSame(), which compares relevant object fields. 
```
private val DIFF_CALLBACK = object : DiffUtil.ItemCallback<Concert>() {
    // The ID property identifies when items are the same.
    override fun areItemsTheSame(oldItem: Concert, newItem: Concert) =
            oldItem.id == newItem.id

    // If you use the "==" operator, make sure that the object implements
    // .equals(). Alternatively, write custom data comparison logic here.
    override fun areContentsTheSame(
            oldItem: Concert, newItem: Concert) = oldItem == newItem
}
```

Becuase your adapter includes your definition of comparing items, the adapter automatically detects changes oto these items when a new PagedList object is loaded. As a result the adapter triggers efficient item animations within your RecyclerView object. 

## Diffing using a diferent adapter type
If you choose not to inherit from PagedListAdapter such as when you're usingn a library that provides its own adapter you can still use the Paging Library adapter's diffing functinoality by working directly with an AsyncPagedListDiffer object. 

## Provide placeholders in your UI
In cases where you want your UI to display a list before your app has finished ftching data, you can show placeholder list items to your users. The pagedList Handles this case by presenting the list item data as null until the data is loaded

PlaceHolders have the following benefits
- support for scrollbars: The pagedList prvoides the number of list items to the PagedListAdapter this information allows the adpater to draw a scrollbar that conveys the full size of the list. As new pages load the scrollbar doesn't jump becuase your list doesn't change size.
- No loading spinner necessary: Becuase the list size is already known, there's no need to alert users that more items are loading. The placeholders themselves convey that information

Before adding support for placeholders, though keep the following preconditions in mind
- Requires a countable data set: Instancw of DataSource form the Room persistence Library can efficiently count their items> if you'r using a custom local storage soution onr a newotrk only data architecture hwoever, it might be expensive or even impossible to determine how many items comprise your data set. 

- Requires adapter to account for unloaded items: THe adpater or presentation mechanism that you use to prepare the list for inflation needs to handle null list items. For example when binding data to a ViewHolder, you need to provide default values to represent unloaded data

- Requires same-size items views: if list items size can change based on their contents such as social networking updates, crossfading between items doesn't look good. We strongly suggest disabling placeholder in this case. 

