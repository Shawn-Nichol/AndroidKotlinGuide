
## Ensure type save args
The recommend way to navigate beteween destinations is to use the Safe Args Gradle plugin. This pluging generates simple object and builder classes that enable type-safe navigation and argument passing between destinations. 

To add SafeArgs to your progject, includ the following classpath in your top level build.gradle file. 
```
buildscript {
    repositories {
        google()
    }
    dependencies {
        def nav_version = "2.3.1"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
```

You must alos apply one of two available plugins

To generate java language cod sutiable for Java or mixed Java and Kotlin modules, add this line ot your app or module's build.gradle. 
```
apply plugin: "androidx.navigation.safeargs"
```

Alternatively, to generat kotlin code suitable for kotlin-only modules add
```
apply plugin: "androidx.navigation.safeargs.kotlin"
```

You must have android.useAndroidX=true in your gradle.properties file as per Migrating to AndroidX

After you enable SafeArgs, the plugin generats code that contains classes and methods for each action youv'e defined. For each actions, safe args also generates a class for each originating destnation, which is the destniation from twhich the action orignates. The generated class names is a combination of originating destination class name and the word "Directions'> For example, if the destination i snamed SpedifyAmountFragmnet, the generated class is naed SpecifyAmountFragmentDirections. The generated class contains a static method for each action define din the originating destination. The generated class contains a staic method for each action defined in the orignating destination. This method takes any defined action parameteres as arguments and returns a NavDirections object that you can pss to navigate(). 

As an example, assume we have a navigation graph with a single ation that connets th originating destination, SpecifyAmountFragment,  to a receiving destination, ConfirmationFragment. 

Safe Args generats a SpecifyAmountFragmentDirections class with a single method, actionsSepcifyAmountFragmentTo confirmationfRagment(0 that retusn a NavDirections object. This returned NavDirections object can bthe be passed directly to navigate(), as shown, 

```
override fun onClick(view: View) {
  val action = SpecifyamountFragmentDirections.actionSepcifyAmountFragmentToConfirmationFragment()
  view.findNavController().navigate(action)
}

```

For more information on passing data between destination with safe Args 


## Use Safe Args to navigate with type-saftey
The recommend way to navigate between destinations is to use the SafeArgs gradle plugin. This plugin generates simple object and builder classses that enable type-safe navigation between destinations. Safe Argss is recommended both for navigating as well as possing data between destinations. 

To Add SAfe Args to your project include  the following classpath in your top level build.gradle file
```
buildscript {
    repositories {
        google()
    }
    dependencies {
        def nav_version = "2.3.1"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
```

You must also apply one of two avaiable plugins

To generate java and mixed Kotlin
```
apply plugin: "androidx.navigation.safeargs"
```

To generate koltin
```
apply plugin: "androidx.navigation.safeargs.kotlin"
```

YOu must have android.useAndroidX=true in your gradle.properties file as per Migrating to AndroidX.

After enabling Safe Args, your generated code contains classes and methods for each action you've defined as well as classes that correspond to each sending and receving destination. 

Safe Args generates a class forr each destination where an action originates. The generated class name adds "Directions" to the originiating destinations class name. Fore example, if the orignating destinations is named SpecifyAmountFragment, the generated class is named SpecifyAmountFragmentDirections. 

The generated class contains a staic method for each action defined in teh originating destinations. this method takes any defined action parameters as arguments and retunss a NavDirectiosn object that you ca pass directly to navigate(). 

## Safe Args example
As an example, assume we have a nvaigation grpah with a single action that connects tow destinationns, SpecifyAmountFragment and ConfirmationFragment. The Confirmationfragment takes a single float parameter that you provide as part of the action. 

Safe Args generates a SpecifyAmountFragmentDirections class with a single emthod, actionSpeicfyAmountFragmentToConfirmationFragment(), and an inner class called ActionSpecifyAmountFragmentToConfimrmationFragment. The inner class is derived from NavDirections and storess the associated action ID and float parameter. The returned NavDirections obejct can then be passed directly to navigate(), as shown in the following example. 

```
Override fun onClick(v: View) {
  val mount Float = ...
  val action =
    .actionSpecifyAmountFragmentToConfirmatoinFragment(amount)
    v.findNavController().navigate(action)
    
    
}
```

For more information on passing data between destinatioons with Safe Args, see Use Safe Args to pass data with type safety
