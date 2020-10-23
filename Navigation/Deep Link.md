# Deep link
The navigation component lets you create two different types of deep links: Explicit and implicit. 


## Explicit DeepLink
An explicit deep link is a single instance of a deep link that uses a PendingIntent to take users to a specific location within your app. You might use an explicit deep link as  part of a notification or an app widget. 

When a user opens your app via an explicit deep link, the task back stack is cleared and replaced with the deep link destination,. When nesting graphs, the start destination from each level of nesting that is, the start destination from each <navigation> element in the hierarchy is also added to the stack. This means that when a user presses the back button from a deep link destinationn, they navigate back up the  navigation stack just as though they entered your app from its entry pint. 

You can use the NavDeepLink Builder class to construct a pendingIntent, as shown in the example below note that if the provided context is not an activity, the constructor uses PackageManager.getLaunchIntentForPackage() as the default activity to launch, if avaiable.

```
val pendingIntent = navDeepLiinkBuilder(context)
  .setGraph(R.navigation.nav_graph)
  .setDestianation(R.id.android)
  .sestArguemtns(args)
  .createPendingIntent()
```

If you have an existing NavController, you can also create a deep link vai NavController.createDeepLink()


## Implicit deep link
An implicit deep link is a URI that refers to a specific destination in an app. A URI is invoked, when a user clicks a link, Android can then open your app to the corresponding destination.

When triggering an implicit deep link, the state of the back stack depends on whether the implicit Intent was launched with the Intent.FLAG_ACTIVITY_NEW_TASK
- If the flag is set, the task back stack is cleared and replaced with the deep link destination. As with explicit deep linking, when nesting graphs, the start destination from each level of nesting - That is, the start destination from each <navigation> element in the hierarchy is also added to the stack. This means that when a users presses the back button from a deep link destination, they navigate back up the navigation stack just as though they entered your app from its entry point. 

- If the flag is not set, you remain on the task stack of the previous app where the implicit deep link was triggered. In this case, the back button takes you back to the previous app, while the UP button starts your app's task on the hierarchical patent destination within your navigation graph.

You can use the Navigation Editor to create an implicit deeep link to a destination as follows:
1. In the design tab of the Navigation Editor, select the destination for the ddep link. 

2. + in the Deep Links section of the Atttributes panel

3. In the add deep link dialog that appears, enter a URI
Note the following:
  - URIs without a scheme are assumed as either http or https. For example ww.google.com matches both http://www.google.com and https://www.google.com
  - Path parameter placeholders in the form of {placeholder_name] match one or more characters. For example, http://www.example.com/user/{id} matchs http://www.example.com/user/4. The navigation component attempts to parse the placeholder values into appropriate types by matching placeholder names to the defined arguemnts that are defined for the deep link destination. If no argument with the same name is defined, a default String type is used for the arguemtn value. You caan use the # wildcard to match 0 or more characters. 
  
  - Query parameter placeholders can be used instead of or in conjunction iwth parameters. For example http://www.example.com/users/{id}?myarg={myarg}
  matches http://www.example.com/users/4?myarg=28
  
 -  Query parameter placeholders for variables defined with default or nullable values are not required to match. For example, http://www.example.com/users/{id}?arg1={arg1}&arg2=    {arg2} matches http://www.example.com/users/4?arg2=28 or http://www.example.com/users/4?arg1=7. This is not the case with path parameters. For example,            http://www.example.com/users?arg1=7&arg2=28 does not match the above pattern because the required path parameter is not supplied.


- Extraneous query parameters do not affect deep link URI matching. For example, http://www.example.com/users/{id} matches http://www.example.com/users/4?extraneousParam=7, even   though extraneousParam is not defined in the URI pattern.


4. optional Check auto verify to require google to verify that you are the owner of the URI. FOr more inoformation see verify android app links

5. Click Add a link icon appears above the selected destination to indicate that destination has a deep link
 
6. Click the text tab to toggle to XML view. A nested <deepLinl> element has been added to the destination
```
deepLink app:uri="https://wwww.google.com"/>
```


## Update Manifest
To enable implicit deep linking, you must also make additions to your app's manifest.xmlm file add a single <nav-graph> element to an activity that point to an existing navigation graph, as shown in the folllwoing example. 

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application ... >

        <activity name=".MainActivity" ...>
            ...

            <nav-graph android:value="@navigation/nav_graph" />

            ...

        </activity>
    </application>
</manifest>

```
When building your project, the Navigation component replaces the <nav-graph> element with generated <intent-filter> element to match all of the deep links in the navigation graph. 

