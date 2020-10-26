
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

## Direction And Args classes
After enabling SafeArgs, build the code. This will generate a Direction and Args class based on the Fragments and Arg data in nav_graph. 

```
<navigation
    ...
    
    <fragment
        android:id="@+id/my_fragment_dest
        android:label="My Fragment">
        
        <action
            android:id="@+id/action_my_fragment_to_details_fragment"
            android:destination="details_fragment"/>
            
    </fragment>
    
    <fragment
        android:id="@+id/details_fragment
        android:label="Details Fragment">
        
        <argument
            android:name="saved_int"
            app:artType="integer"
            android:defaultValue="0"/>
    </fragment>

```


### Dircections:
Takes the name of the destination and combines it with Directions,  MyFragment will be named MyFragmentDirections. This class takes any defined action parameters as arguments and returns a NavDirections object that you can pass to navigate(). 

Safe args also generates a class for each originating destnation, which is the destniation from witch the action orignates. The generated class names is a combination of originating destination class name and the word "Directions" For example, if the destination is named SpedifyAmountFragmnet, the generated class is named SpecifyAmountFragmentDirections. This method takes any defined action parameteres as arguments and returns a NavDirections object that you can pass to navigate(). 

```
override fun onClick(view: View) {
  val action = MyFragmentDirections.actionMyFragmentToDirection(5)
  view.findNavController().navigate(action)
}

```

### Args
Takes the name of the argument in nav_graph and adds Args to the name, MyData becomes MyDataArgs

```
val args: MyDataArgs = MyDataArgs.fromBundle(requireArguments())
args.saved_int

```


