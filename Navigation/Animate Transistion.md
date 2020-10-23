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

