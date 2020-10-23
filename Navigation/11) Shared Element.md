# Shared Element transistion between destinations
When a view is shared between two destinations, you can use a shared element transition to define how the view transitions when navigating from one destination to the other. 

Note: When using a shared element transition, you should not use the Animation Framework(enterAnim, exitAnim). Instead, you should be using only the Transistion Framework for setting your enter and exit transitions. 

Shared elements are supplied programatically rather than through your navigation XML file, Activity and fragment destinations each hava a subclass of the Navigator.Extras interface that accepts additional options for navigation, including shared elements. 

## transitionName
The shared views need a transitionName, to simplify operation the name should be the same for each shared view. 

FragOne
```
<TextView
    android:id="@id/frag_one_tv
    ...
    android:transitionName="my_transition"
    />
```

FragTwo
```
<TextView
    android:id="@id/frag_two_tv
    ...
    android:transitionName="my_transition"
    />
```



### FragmentNavigator.Extras
The FragmentNavigator.Extras class allows you to map shared elements from one destination to the next by their transition name, similar to using FragmentTrascation.addSharedElement(). You can then pass the extras to navigate().

View: The View ID for the TextView in FragOne.
String: The transitionName set in FragTwo.

```
val extras = FragmentNavigatorExtras('view1 to "hero_image")

view .findNavController().navigate(
  R.id.confirmationActiono,
  null, // Bundle of args
  null, // NavOptions
  extras)
```

## SharedElementEnterTransition
Returns the Transition that will be used for shared elements trasferred into the content scene. Typical transitions will affect size and locatin, such as Changedbounds.

objectThe transition to use for shared elements transferred into the content Scene.
```
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        sharedElementEnterTransition = TransitionInflater.from(context).inflateTransition(android.R.transition.move)
    }
```
the android R.transition.move animation handles a lot transition, but you can insert a custom transition if you want. 
