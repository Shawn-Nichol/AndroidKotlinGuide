# Navigate between fragmetns uisng animations
The Fragment API provides two ways to use motion effects and transformations to visually connect fragmetns duing navigation. One of these is the Animation Freamework, which uses both Animation and animator. THe other itheh Transition Framework, which includes hared element transistions. 

Note: In this topic, we use the term animation to descibe effects in teh Animation Framework, and we use the term transitioin to describe effects in the Transition Framework. These two frameworks are mutually exclusive and should not be used at the same time. 

You can specify custom effects for entering and exiting fragments and for tansitions of shared elements between fragments. 
- An enter effect determines how a fragment enters the screen. For example, you can creae an effect to slide the fragment in from the edge of the screen when you navigate to it. 
- An exit effect determines how a fragment exits the screen. For example, you can create an effect to fade the fragment out when navigating away.
- A shared element transistion determines how a view that is shared between two fragments moves between them. For example, an image displayed in an ImageView in fragment A transitions to fragment B once B becomes visible. 

## Set animations
Firs you need to create animations for your enter and exit effects, which are run when navigating to a new fragment. You can define animation as tween animation resources. These resources allow you to define how fragments should rotate,stretch, fade, and move during the animation. For example, you might want the current fragment to fade out and the new fragment to slide in from the right edge of the screen. 

These animation are defined in the res/anim directory

```
<!-- res/anim/fade_out.xml -->
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="1"
    android:toAlpha="0" />
```

```
<!-- res/anim/slide_in.xml -->
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="100%"
    android:toXDelta="0%" />
```

Note: it is strongly recommend to use transitions for effects that invlove more than one type of animation as there are known issues with using nested AnimationSet instances.

You can also specify animation for the enter and exit effects that are run when popping the back stack, which can happen when the user taps the UP or back button. These are called the popEnter and popExit animations. For example, when a user pops back to a previos screen, you might want the current fragment to slide off the right edge of the screen and the previos fragment to fade in. 

These animation s can be defined as follows. 
```
<!-- res/anim/slide_out.xml -->
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="0%"
    android:toXDelta="100%" />
```

```
<!-- res/anim/fade_in.xml -->
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="0"
    android:toAlpha="1" />
```
Once you've defined your animations, use them by calling FragmentTrasaction.setCustomAnimations(), passing in your animation resrouces by thier resource ID. 
```
val fragment = FragmentB()
supportFragmentManager.commit {
    setCustomAnimations(
        enter = R.anim.slide_in,
        exit = R.anim.fade_out,
        popEnter = R.anim.fade_in,
        popExit = R.anim.slide_out
    )
    replace(R.id.fragment_container, fragment)
    addToBackStack(null)
}
```

## Set Transistions
You can also use transistions to define enter and exit effects. These transistions can be defined in XML resource files. For example, you migh want the current fragment to fade out and the new fragment to slide in from the right edge of the screen. These transistions can be defined as follows. 
```
<!-- res/transition/fade.xml -->
<fade xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"/>
```

```
<!-- res/transition/slide_right.xml -->
<slide xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:slideEdge="right" />
```
Once you've defined your transitions, apply them by calling setEnterTransition() on the entering fragment and setExitTransition() on the exiting fragment, passing in your inflated transition resources by their resource ID. 

class FragmentA : Fragment() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val inflater = TransitionInflater.from(requireContext())
        exitTransition = inflater.inflateTransition(R.transition.fade)
    }
}

class FragmentB : Fragment() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val inflater = TransitionInflater.from(requireContext())
        enterTransition = inflater.inflateTransition(R.transition.slide_right)
    }
}

Fragments suporrrt AndroidX transitions

## Use shared element transitions
Part of the Transition Framework, shared element transitions determine how corresponding views move between two fragments during a fragment transitions Determine how corresponding views move between two fragments during a fragment ttansition . For exmaple, you migh want an image displayed ina an ImageView on Fragment A to transition to fragment B once B becomes visible. 

At high level, here's how to make a fragment transition with shared elements. 
1) Assign a unique transitino name to each shared element view. 
2) Add shared element views and transition names to the FragmentTransaction
3) Set a shared element transition animation. 

First you must assign a unque transition name to each shared element view to allow the veiws to be mapped from one fragemtn to the next. Set a transtion mame on shared elements in each fragmen tlayout using ViewCompat.setTransitionName(), which provides compatibility for API level 14 and above.
```
class FragmentA : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        ...
        val itemImageView = view.findViewById<ImageView>(R.id.item_image)
        ViewCompat.setTransitionName(itemImageView, “item_image”)
    }
}

class FragmentB : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        ...
        val heroImageView = view.findViewById<ImageView>(R.id.hero_image)
        ViewCompat.setTransitionName(heroImageView, “hero_image”)
    }
}
```
To include your shared elements in the fragment transition, your FragmentTransaction must know how each shared element's views map from one fragment to the next. Add each of your shared elemetns to your FragmentTransaction by calling FragmentTransaction.addSharedElement(), passing in the view and the transistion name of the corresponding view in the next fragment.
```
val fragment = FragmentB()
supportFragmentManager.commit {
    setCustomAnimations(...)
    addSharedElement(itemImageView, “hero_image”)
    replace(R.id.fragment_container, fragment)
    addToBackStack(null)
}
```
To specify how your shared elements transition from one fragment to the next, you must set an enter transition on the fragment being navigated to. Call Fragment.setSharedElementEnterTransition() in the fragment's onCreate() method. 
```
class FragmentB : Fragment() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        sharedElementEnterTransition = TransitionInflater.from(requireContext())
             .inflateTransition(R.transition.shared_image)
    }
}
```

The Shared transistion is defined as follows
```
<!-- res/transition/shared_image.xml -->
<transitionSet>
    <changeImageTransform />
</transitionSet>
```

All subclasses of Transition are supported as shared element transitions. If you want to create a custom Transition, see Create a custom transition animation. changeImageTransform, used in the previous example, is one of the available prebuilt translations that you can use. You can find additional Transition subclasses in teh API reference for the Transition class. 

By defeault, the shared element enter transition is alo used as the return transition for shared elements. The return transition determines how shared elements transition back to the previos fragment when the fragment transaction is popped off the back stack. If you'd like to specify a different return trnasition, you can do so using Fragment. setSharedEleemtnReturnTransition() inthe fragment's onCreate() method. 

Postponing trnasitions
In some cases, you might need to pospone your fragment transition for a short period of time. For examle, you might need to wait until all views inteh entering fragment have been measured and laid out so that android can accurately capture their start and end states for the transition. 

Additionally, your transition might need to be posponed until some necessary data has been loaded. For example, you might need to wait until images have been loaded for shared elements. Otherwise, the transition might be jarring if an image finishes loading during or after the transition. 

To pospone a transition, you must first ensure that the fragmen transaction allows reordering of fragment state changes. To allow reordering fragment state changes, call FragmentTransactioon.setReorceringAllowed(). 

```
val fragment = FragmentB()
supportFragmentManager.commit {
    setReorderingAllowed(true)
    setCustomAnimation(...)
    addSharedElement(view, view.transitionName)
    replace(R.id.fragment_container, fragment)
    addToBackStack(null)
}
```

To pospone the enter transition, call Fragment.posponeEnterTransition(0 in the entering fragment's onViewCreated() method
```
class FragmentB : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        ...
        postponeEnterTransition()
    }
}
```

Once you've loaded the data and are ready to start the transition, call Fragment.startPosponedEnterTransition(0. The following example uses the Glide library to load an image into a shared ImageView, postponing the correponsind transition until image loading has completed. 
```
class FragmentB : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        ...
        Glide.with(this)
            .load(url)
            .listener(object : RequestListener<Drawable> {
                override fun onLoadFailed(...): Boolean {
                    startPostponedEnterTransition()
                    return false
                }

                override fun onResourceReady(...): Boolean {
                    startPostponedEnterTransition()
                    return false
                }
            })
            .into(headerImage)
    }
}
```

When dealing with cases such as a user's slow internet connection, you might need the posponed transition to start after a certain amount of time rather than waiting for all of the data to load. For these situations, you can instead call Fragment.postponeEnterTransition(long, TimeUnit) in the entering fragment's onViewCreated method, passing in the duration and the unit of time. The postponed then automatically starts once the specified time has elapsed. 

## Use shared element transition with a RecyclerView. 
Pospned enter transitions should not start until all views in teh entering fragment bave geen measured and laid out. When using a RecyclerView, you must wait for any data to load and for the RecyclerView items to be ready to draw before starting the transition. 
```
class FragmentA : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        postponeEnterTransition()

        // Wait for the data to load
        viewModel.data.observe(viewLifecycleOwner) {
            // Set the data on the RecyclerView adapter
            adapter.setData(it)
            // Start the transition once all views have been
            // measured and laid out
            (view.parent as? ViewGroup)?.doOnPreDraw {
                startPostponedEnterTransition()
            }
        }
    }
}
```

Notice that a ViewTreeOBserver.OnPreDrawListener is set on the parent of the fragment view. This is to ensure that all of the fragment's views have been meaured and liad out and are therfore ready to be drawn before beginning the pospned enter transition. 

Note: When using a shared eleemtn transistion from a fragment using a RecyclerView to another fragment, you must still postpone the fragment usinga RecyclerView to ensure that the returning shared element transition functions correctly when popping back to the RecyclerView. 

Another point to consider when using shared element transitions with a RecyclerView is that you cannot set the transition name in the RecyclerView item's XML layout becuase an arbitrary number of items share that layout. A unique transition anme must be assigned so that the transition animation uses the correct view. 

You can give each item's shared eleemtn s aunique transitions name by assigning them when the viewHOlder is bound. For exmaple, if the data for each item includes a unique ID, it could be used as the transition name. 
```
class ExampleViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    val image = itemView.findViewById<ImageView>(R.id.item_image)

    fun bind(id: String) {
        ViewCompat.setTransitionName(image, id)
        ...
    }
}
```
