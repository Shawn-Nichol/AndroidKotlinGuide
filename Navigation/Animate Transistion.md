## Animate transitions between destinations

The navigation component lets you add both property and view animation to actions.Navigation includs several default animation. 

Add animation
1. The navigation editor, click on the action where the animation should occur. 

2. The Animations section of the Attributes panel, click the dropdown arrow next to the animation you'ld like to add. You can choose between the following types. 
- Entering a destination.
- Exiting a destination.
- Entering a destinatino via a pop action.
- Exiting a destination via a pop action. 

3. Choose an animation from the list. 

XML will now look like this. 
```
<action
    android:id="@+id/confirmationAction"
    app:destination="@id/confirmationFragment"
    app:enterAnim="@anim/slide_in_right"
    app:exitAnim="@anim/slide_out_left"
    app:popEnterAnim="@anim/slide_in_left"
    app:popExitAnim="@anim/slide_out_right" />
```

## Shared eleemtn transistion between destinations
Wehn a view is hared between two destinations, you can use a shared element transition to define how the viw transitions when navigating from one destination to the other. Shared eleemnt transitions are part of the Transition Framework. 

Note: When using a shared element transition, you should not use the Animation Framework(enterAnim, exitAnim). Instead, you should be using only the Transistion Framework for setting your enter and exit transitions. 

Shared elements are supplied programmatically rather than throught your navigation XML file Activity and fragment destinations each hava a subclass of the Navigator.Extras interface that accepts additional options for navigation, including shared elements. You can pass these Extras when call navigate()

## Shared element transitions to a fragment destination
The GragmentNavigator.Extras class allows you to map shared elements from one destination to the next by their transition name, similar to using FragmentTrascation.addSharedElement(). You can then pass the extras to navigate().

```
val extras = FragmentNavigatorExtras('view1 to "hero_image")

view .findNavController().navigate(
  R.id.confirmationActiono,
  null, // Bundle of args
  null, // NavOptions
  extras)
```
