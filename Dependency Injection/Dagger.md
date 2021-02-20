# Dagger basics
Manual dependency injection or service locators ina n Android app can be proglematic depending on the size of your project. You can limit your project's complexity as it scalbes up by using Dagger to manage dependencies

Dagger automatically generrates code that mimics the code you would otherwise have hand-written. Beuase the code is generated at compile time, it's traceable and more perfromant than other reflection based solutions such as Guice. 

# Benefits of using Dagger
Dagger frees you from wirting tedious and error-prone boilerplate code.
- Generating the AppContainer code that you manually implemented in the manual DI section 
- Creating factories for the classes available in the application graph. This is how dependencies are satisfied internally. 
- Deciding whether to reuse a dependency or create a new instance through the use of scopes. 
- Creating continaers for specific flows as you did with the login flow in the previous section using Dagger subcomponents. This improves your app's perfromance by releasing objects in memory when they're no longer needed. 

Dagger automatically does all of this at build time as long as you declare dependencies of a class and spedify how to satisy them using annotations. Dagger generates code dimilar to what you would have written manually internally inthe graph Dagger generates a facotry type class that is uses internally to egget instances of that type. 


At build time, Dagger walks through your doe and 
- Builds and validates depenedency graphs, ensuring that 
  - Every object's depenedencs can be satisfied, so there are no runtime exceptions
  - No dependency cycles exist, so there are no infiinite loops. 
- Generate the classes that are used at runtime to create the actual objects and their dependencies. 

## A simple use case in Dagger: Gennerating a facotry
```
class UserRepository(
  private val localDataSource: UserLocalDataSource,
  private val remoteDataSource: UserRemoteDataSource
) { ... }
```

Add an @Inject annotation to the UserRepository constructor so Dagger knows how to create a UserRepository.
```
// @Inject lets Dagger know how to create instances of this object
class UserRepository @Inject constructor(
  private val localDataSource: UserLocalDataSource, 
  private val remoteDataSource: UserRemoteDataSource
) { ... }
```
This tells dagger to
1. How to create a UserRepository instance with the @Inject annotated constructor
2. What its dependencies are: UserLocalDataSource, and userRemoteDataSource.

Now Dagger knows how to create an instance of UserRepository, but it doesn't know how to create its depenedencies. If you annotate the other classes too Dagger knows how to creat them. 

```
@Inject lets Dagger know how to create instances of these objects
class UserLocalDataSource @Inject constructor() {... }
class userRemoteDataSource @Inject constructor() {... }
```

## Dagger components
Dagger can create a graph the dependencies in your project that it can use to find out where it should get those dependencies when they are needed. To make Dagger do this, you need to create an interface and annotate it with @Component. Dagger creates a container as you would have done with manual dependency injection. 

Inside the @Component. Dagger creates a constiner as you would have done with manual dependency injection. 

Inside the @Component interface, you can define functions that return instances of the classes you need (i.e. UserRepository). @Component tells Dagger to generate a container with all the depndencies required to satisfy the types it exposes. This is called a Dagger component; it contains a graph that consists of the objects that Dagger knows how to provide and their repspective dependencies. 

```
// @Component makes Dagger create a graph of depedencies
@Component
interface ApplicationGraph {
  // The return type of functions insde the component interface is what can be provided from the container. 
  fun repository(): UserRepository
}
```

When you build the project, Dagger generates n implementations off ApplicationGraph interface for you: DaggerApplicationGraph. With its annotation processor. Dagger creates a dependency graph that consists of the relationships between the three classes(UserRepsoitory, UserLocalDataSource, UserRemoteDataSource) with only one entry point getting a UserRepsotiroy instance you can use it as follows. 

```
// Create an instance of application graph
val applicationGraph: ApplicationGraph = DaggerApplicationGraph.create()
// Grab an instance of UserRepository from the applicaion graph
val userRepository: UserRepository = applicationGraph.repository()

```

Dagger creates a new instance of the UserRepository everytime it is requested. 
```
val applicationGraph: ApplicationGraph = DaggerApplicationGraph.create()

val userRepository: UserRepository = applicationGraph.repository()
val userRepository2: UserRepository = application.repository()

assert userRepository != userRepository2)
```

Sometimes you need to have unique instance of a dependency in a container. You might want this for several reasons
1. You want other types that have this type as a dependency to share the same instance, such as multiple ViewModel objects in teh login flow using the same LoginUserData. 
2. An object is expensive to create an you don't want to create a new instance every tiem its delcared as a dependency 

In the example, you might want to have a unique instance of UserRepository available in the graph so that every time you ask for a UserRepository, you always get the same instance. This is useful in your example becuase in a real-life application with a more complex applicatoin graph, you migh have multiple Viewmodel objects depending on UserRepository and you don't want to create a new instance of UserLocalDataSource and UserRemoteDataSource every tiem userRepostiory needs to be provided. 

In manual depenedcy injection you do this by passing in the same instance of UserReposiotry to the construvtos of the ViewModel classes; but in Dagger, becuase you are not writing that code maunaly, you have to let Dagger know you want to use the same instance. This can be done with scope annotations. 

## Scoping with DAgger
You can use scope annotations to limit the lifetime of an object to the lifetime of its components. THis means that the same instance of a dependency is used everytime that type needs to be provided. 

To have a unique instance of a UserRepsotiry when you ask for the repositroy in ApplicationGraph, use the same scope annotation forothe @componet interface and userRepsitory. Youcan use the @Singleton annotation tahtalready comes with the javax.inejct package that Dagger uses. 

```
// Scope annotations on a @Componetn interface informs dagger that classes annotated with this annotation are boudn to the life of the graph and so the same instane of that type // is provided every time the type is requested
@Singleton
@Component
interface ApplicationGraph {
  fun repository(): UserRepository
}

// Scope this class to a component using @Singleton scope (i.e. ApplicationGraph)
@Singleton
class UserRepository @Inject constructor(
  private val localDataSource: UserLocalDatasource
  private val remoteDataSource: RemoteDataSource
) { ... }
```

Altenratively, you can createa and use a custom scope annotation. You cancreate a scope annotations as follows. 
```
// Creates MyCustomScope
@Scope
@MustBeDocuemented
@Retention(value = AnnotationRetetion.RUNTIME)
annotation class MyCustomScope

```

Then you can use it as before
@MyCustomScope
@Component
interface ApplicationGraph {
  fun repository(): UserRepository
}


@MyCustomScope
class UserRepository @Inject constructor(
  private val localDataSoure: UserLocalDataSource, 
  private val service: UserService
) { ... }
```

In both cases, the object is provided with the same scope used to annotate the @Component interface. Thus everytime you call appliationGraph.repository(), you get the same instance of UserRepository

```
val application ApplicationGraph = DaggerApplicatoinGraph.create()

val userRepository: UserRepository = ApplicationGraph.repository()
val userRepository2: UserRepository = AppplicationGraph.repository()

assert(userRepository == userRepository2)
```
