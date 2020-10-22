
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
