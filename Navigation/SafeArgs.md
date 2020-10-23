
## Ensure type save args
The recommend way to navigate beteween destinations is to use the Safe Args Gradle plugin. This pluging generates simple object and builder classes that enable type-safe navigation and argument passing between destinations. 

To add SafeArgs to your progject, include the following classpath in your top level build.gradle file. 
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

You must also apply one of two available plugins

Java and Kotlin
```
apply plugin: "androidx.navigation.safeargs"
```

Kotlin
```
apply plugin: "androidx.navigation.safeargs.kotlin"
```

After enabling SafeArgs, the plugin generats code that contains classes and methods for each action you've defined. Safe args also generates a class for each originating destnation, which is the destniation from witch the action orignates. The generated class names is a combination of originating destination class name and the word "Directions" For example, if the destination is named SpedifyAmountFragmnet, the generated class is named SpecifyAmountFragmentDirections. This method takes any defined action parameteres as arguments and returns a NavDirections object that you can pass to navigate(). 

As an example, assume we have a navigation graph with a single action that connets the originating destination, SpecifyAmountFragment,  to a receiving destination, ConfirmationFragment. 

Safe Args generats a SpecifyAmountFragmentDirections class with a single method, actionsSepcifyAmountFragmentTo confirmationFragment() that retuns a NavDirections object. This returned NavDirections object can be passed directly to navigate(), as shown, 

```
override fun onClick(view: View) {
  val action = SpecifyamountFragmentDirections.actionSepcifyAmountFragmentToConfirmationFragment()
  view.findNavController().navigate(action)
}

```

