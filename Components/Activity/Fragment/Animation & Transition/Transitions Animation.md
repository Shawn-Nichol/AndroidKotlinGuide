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


