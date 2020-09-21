# Transitions
There are two steps in creating a fragment transition

## 1) You can use transistions to define enter and exit effects. Define these transistion in an XML file.
```
<?xml version="1.0" encoding="utf-8"?>
<transitionManager xmlns:android="http://schemas.android.com/apk/res/android">
    <fade xmlns:android="http://schemas.android.com/apk/res/android"
        android:duration="@android:integer/config_shortAnimTime"/>
</transitionManager>
```

## 2) Transition added to Transaction
Once the transitions is defined apply them by calling setEnterTransition() on the entering fragment setExitTransition() on the exiting fragment, passing in your inflated transition resources by their resource ID. 

```
class FragmentA : Fragment() {
  override fun onCreate(onSavedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val inflater = TransitionInflater.from(requirecontext())
    exitTransition = inflater.inflateTransition(R.transition.fade)
  }
}

class FragmentB : Fragment() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    val inflater = TransitionInfalter.from(requireContext())
    enterTransition = inflater.inflateTransition(R.transition.slide_right)
  }
}
```

# Shared Element Transistion
## 1) Transition Name
Provide a transistion name for the start view and end view in the XML layout, the name doesn't need to match but will make the code more readable if they do. 
```
android:transitionName="transition_name"
```

## 2) FragmentTransaction
In the fragment transaction use the addSharedElement(sharedElement, name)
sharedElement: A view in a disappearing Fragment to match with a View in a appearing fragment
name: String the transitionName for a view in an appearing Fragment to match to the shared element. 
```
ft.addToBackStack(null)
  .addSharedElemenet(startingView, "transition_name")
  .replace(R.id.fragment_container, fragmentTwo)
  .commit()
```
Be sure to include the addSharedElement before replace. 

## 3) Set Transition
In onCreate of the end fragment set the transition that will be used for shared elements transferred into the content scene with setSharedElementEnterTransition. 
```
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  sharedElemenetEnterTransition = TransitionInflater.from(requirecontext())
    .inflateTransition(R.transition.my_transition)
}
```

## 4) Transition file
Create the transistion file, there are number
```
<?xml version="1.0" encoding="utf-8"?>
<transitionSet xmlns:android="http://schemas.android.com/apk/res/android">
    <changeBounds />
</transitionSet>
```
