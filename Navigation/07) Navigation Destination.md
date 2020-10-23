# Navigation to a destination

Navigating to a destination is done using a NavController, an object that manages app navigation within a NavHost. Each NavHost has its own corresponding NavController.To retrieve the NavController for a fragment, activity, or view. 

Kotlin
- Fragment.findNavController()
- Navigtion.findNavController(Activity, @IdRes int viewId)
- Navigation.findNavController(view)

After retriveing the NavController, you can call one of the overloads of navigate() to navigate between destinations. 

## Navigate using ID
navigate(int) takes the resource ID or either an action or a destination.
```
view.TransactionButton.setOnClickListner { view -> 
  view.findNavController().navigate(R.id.viewTransactionsAction)
```

For buttons, you can also use the Naviagtion class's createNavigateOnClickListener() convenience method to navigate to a destination, as shown in the following example.

```
button.setOnClickListener(Navigation.createNavigatieOnClickListener(R.id.next_fragment, null)
```

## Provide navigation options to actions
When you define an action in the navigation graph, Navigation generates a corresponding NavAction class, which contains the configurations defined for that action, including the following

- Destination: The resource ID of the target destination.
- Default arguments: An android.os.Bundle containing default values for the target destination, if supplied. 
- Navigation options: Navigation options, represented as NavOptions. This class contains all of the special configuration for transitioning to and back from the target destination, including animation resource configuration, pop behavior, and whether the destination should be launched in single top mode. 

Example consisting of two screens along with an action to navigate from one to the other. 
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:id="@+id/nav_graph"
            app:startDestination="@id/a">

    <fragment android:id="@+id/a"
              android:name="com.example.myapplication.FragmentA"
              android:label="a"
              tools:layout="@layout/a">
        <action android:id="@+id/action_a_to_b"
                app:destination="@id/b"
                app:enterAnim="@anim/nav_default_enter_anim"
                app:exitAnim="@anim/nav_default_exit_anim"
                app:popEnterAnim="@anim/nav_default_pop_enter_anim"
                app:popExitAnim="@anim/nav_default_pop_exit_anim"/>
    </fragment>

    <fragment android:id="@+id/b"
              android:name="com.example.myapplication.FragmentB"
              android:label="b"
              tools:layout="@layout/b">
        <action android:id="@+id/action_b_to_a"
                app:destination="@id/a"
                app:enterAnim="@anim/nav_default_enter_anim"
                app:exitAnim="@anim/nav_default_exit_anim"
                app:popEnterAnim="@anim/nav_default_pop_enter_anim"
                app:popExitAnim="@anim/nav_default_pop_exit_anim"
                app:popUpTo="@+id/a"
                app:popUpToInclusive="true"/>
    </fragment>
</navigation>
```
When the navigation graph is inflated, these actions are parsed, and corresponding NavAction objects are generated with the configurations defined in the graph. For example, action_b_to_a is defined as navigation from destination b to destination a. The action includes animations along with popTo behavior that removes all destinations from the backstack. All of these settings are captured as NavOptions and are attached to the NavAction.

To follow this NavAction, use NavController.navigate(), passing the ID of the action, as shown
```
findNavController().navigate(R.id.action_b_to_a)
```

## Navigate using DeepLinkRequest
You can use navigate(NavDeepLinkRequest) to navigate directly to an implicit deep link destination.
```
val request = NavDeepLinkRequest.Builder
  .fromUri("android-app://androidx.navigation.app/provile".toUri())
  .build()
findNavControler().navigate(request)

```
In addition to Uri, NavDeepLinkRequest also supports deep links with actions and MIME types. To add an action to the request, use fromAction() or setAction(). To add a MIME type to a request, use from MimeType() or setMimeType(). 

For a NavDeepLinkRequest to properly match an implicit deep link destination, the URI action, and MIME type must all match the NavdeepLink in the destination. URIs must match the patteren, the actions must be an exact match and the MIME types must be related. 

Unlike navigation using action or destination IDs, you can navigate to any deep link in your graph, regarcless of whether the destination is visible. You can navigate to a destination on the current graph or a destination on a completely differen graph. 

When navigatin using NavDeepLinkRequest, the back stack is not reset. This behavior is unlike other deep link navigation, where the back stack is replaed when navigating. popUpTo nad popUp ToInclusive still remove destinations from the back stack just as through you had navigated usingan ID. 

## Navigation and the back stack
Android maintans a back stack that contains the destinations you've visted. The first destination of your app is placed on the stack when the user opens the app. Each call to the navigate() method puts another destination on top of the stack. Tapping Up or back calslt the NavController.navigtaeUp() and NavController.popBackStack() methods, repsectiviely, to remove the top destination off of the stack. 

NavController.popBackStack(0 returns a boolean indicating whether it successfully popped back to another destination. The most common case when this returns false is when you manually pop the start destinatoin of your graph. 

When the method retuns false, NavController.getcurrentDestination() returns null. You are responsible for either navigating to a new destination or handling the pop by calling finish() on your activity, as shown in the following example. 

```
if(!navController.popBackStack()) {
  // call finsish() on your activity
  finish()
}
```

## popUpTo and popUpToInclusive

When navigating using an action, you can optionally pop additional destinations off of the back stack. For example, if your app has an intital login flow, once a user has logged in, you should pop all of the login-related destinations off of the back stack so that the back button doesn't take users back inoto the login flow. 

To pop destinations when navigating from one destination to another, add an app:popUpTo attribute to the associated<action> elemetn. app:popUpTo tells the Navigation library to pop some destinations off of the back stack as part of the call to navigate(). The attribute values is the ID of the most recent destination that should remain on the stack. 

You can also include app:popUpToInclusive="true" to indicate that the destination specifeid in app:popUpTo should also be removed from the bck stack. 

## popUpTo example: circular logic
Lets say that your app has three destinations A, B, and C along with action that lead from A to B, B to C and C back to A. 

with each navigation action a destination is added to the back stack. If you were to navigate repeatedly through this flow, you back stack woudl thencontain multiple sets of each destination A, B, C, A, B, C, A. To avoid this repetion, you can specify app:popUpTo and app:popUpToInclusive in the action that takes you from destination C to destination A, as shown in the following. 

```
<fragment
    android:id="@+id/c"
    android:name="com.example.myapplication.C"
    android:label="fragment_c"
    tools:layout="@layout/fragment_c">

    <action
        android:id="@+id/action_c_to_a"
        app:destination="@id/a"
        app:popUpTo="@+id/a"
        app:popUpToInclusive="true"/>
</fragment>
```

After reaching destination C, the back stack contains one instance of each destination (A, B, C). When navigating back to destinatino A, we also popUpTo A, which means that we remove B and C from the stack while navigating. With app:popUpToInlusive="true", we alos pop that first A off of the stack, effeectively clearing it. Notice here that if you don't use app:popUpotIncusive, your back stack would contain tow instances of destination A. 

