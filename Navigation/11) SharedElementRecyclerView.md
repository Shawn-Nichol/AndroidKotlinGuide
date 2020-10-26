## Nav_Graph
Add action to fragments in navigation

nav_graph
```
<navigation
  ...
  
  <fragment
    android:id="@+id/list_dest
    android:label="list_fragment"
    tools:layout="@layout/list_fragment">
    
    <action
      android:id="@+id/action_list_to_details"
      app:destination="@id/details_dest/>
    
  </fragment>
  
    <fragment
    android:id="@+id/list_dest
    android:label="list_fragment"
    tools:layout="@layout/list_fragment">    
  </fragment>
  ```
  
  ## setTransition name
  In the details_fragment.xml set the transition name on view. 
  ```
          <TextView
            android:id="@+id/tv_dest_two_item_details"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            ...
            android:transitionName="rv_transition"/>
  ```
  
  
## onBindViewHolder
Update the onBindViewHolder() method in the RecyclerView Adapter to set the transition name and set and on click listener with navigation built in. 
```
override fun  onBindViewHolder(holder: ItemViewHolder, position: Int) {
  val item: Int = dataSet[position]
  holder.tv.text = item.toString()
  holder.tv.transitionName = item.toString()
  
  holder.tv.setOnClickListener {
    val bundle: Bundle = bundleOf("rvPosition" to dataSet[position])
    val extras: FragmentNavigator.Extras = FragmentNavigatiorExtras(holder.tv to "rv_transition")
    Naviation.findNavController(holder.tv).navigate(R.id.action_list_to_details, bundle, null, extras)
  }
}
```

## SetupTransition
In the detals fragment set shared transition element in onCreate()

```
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedinstanceState)
  
  val name = arguments?.getInt("rvPosition") ?: -1
  sharedElementEnterTransition = TransitionInflater.from(context).inflateTransition(android.R.transition.move)
}
```


## PostPone the EnterTransistion(Button Back press)
PostPone the enter transition, the transition will run after all the views are built.  Update the RecyclerView settings
```
recyclerView.apply {
  ...
  
  // Dleay transition on backpress
  postponeEnterTransition()
  viewTreeObserver.addOnPreDrawListener {
    startPostponeEnterTransition()
    true
  }
}
```
  
