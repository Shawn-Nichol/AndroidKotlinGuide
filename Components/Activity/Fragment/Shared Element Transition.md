## Assign transitionName
Assign a transition name to both views, they can be different but is easier if they use the same name. 

FragOne
```
<TextView
  android:id="@/tv_frag_one"
  ...
  android:transitionName="transition_text"
 />

```

FragTwo
```
<TextView
  android:id="@/tv_frag_two"
  ...
  android:transitionName="transition_text"
/>
```


## addSharedElement()
Used with custom Transitions to map a View from a removed or hidden Fragment to a view from shown or added Fragment.

sharedElement: A view in a disappearing Fragment to match witha View in an appearing Fragment. 
name: String: The transitionName for a view in an appearing Fragment to match to the shared Element. 

```
actiivty!!.supportFragmentManager.beginTransaction().apply {
  replace(R.id.frag_container, fragTwo)
  addToBackStack(null)
  addSharedElement(text_view, "transition_text"
  commit()
}
```


## setSharedElementTransition()
Sets the Transition that will be used for shared elements transferred into the content scene. Typical Transitions wil affect size and location, such as ChangeBounds. A null value will cause transferred shared elements to blink to the final poition

transition: The Transition to use for shared elements transferred into the content Scene. 

FragTwo
```
override fun onCreate(...) {
  super.onCreate(savedInstanceState)
  setSharedElementEnterTransition(TransitionInfalter.from(getContext()).inflateTransition(android.R.transition.move))
}

```
