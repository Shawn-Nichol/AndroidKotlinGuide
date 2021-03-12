# Dagger
Dagger automatically generates code that mimics the code you would otherwise have written. Becuase the code is generated at compile time, it's traceable. Dagger frees you from writing tedious and error-prone boilerplate code. Dagger will also make testing easier. 


`Manual Dependency Injection:` Refers to manually enter the objects need to start a class. 

At build time, Dagger walks through your code and
- Builds and validates dependency graphs, ensuring that
  -   Every object's dependencies can be satisfied.
  -   No dependency cycels exist, so there are no infinite loops.
- Generate the classes that are used at runtime to create the acutal objects and theier dependencies. 



## Components
`@Component:` An Interface that tells Dagger creates a graph of dependencies in your project that it can use to find out where in should get those dependencies when they are needed. </br>
`SubComponent:` are compoents that inherit and extend the object graph of a parent component. Thus , all objects provide din the parent component will be cprovidedin the subcomponent too. </br>

`DaggerGraph:` The implementation of the application graph is automatically generated by the annotation processor. The genreated class is called Dagger{ComponentName}. </br>

`@BindsInstance:` Objects that are constructoed outside of the graph (eg, context)

## Scoping
You can use scope annotations to limit the lifetime of an object to the lifetime of its component. This means that he same instance of a dependency is used every time that type needs to be provide. </br>

`@Single:` Scope only allows the component to be created once, all of the appcompoonents will only be created once.  </br>
`@ApplicationScope:` </br>
`@LoggedUserScope:` </br>
`@ActivityScope:` </br>


## Injection
`@Inject:` 
- Use constructor injection to add types to a graph whenever it's possile. 
- Use Inject to provide an instance of an object from the graph. 

## Modules
`@Modules:` A Dagger module is a class, where you can define dependencies. In order for the graph to know about the modules you have to provide @component interface with the module name. 
`Binds:` Tells Dagger which implementation an interface should have. 
`@Provide:` Use Provide to tell Dagger how to provie a class that our project doesn't own. 



