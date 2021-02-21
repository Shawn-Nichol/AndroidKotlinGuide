## Dependencies
```
apply plugin: 'kotlin-kapt'

dependencies {
    implementation 'com.google.dagger:dagger:2.x'
    kapt 'com.google.dagger:dagger-compiler:2.x'
}
```

In android, you usually create a Agger graph that lives in your application class becuase you want tn instance of the graph to be in memory as long as the app is running. In this way, the raph is attached to the lifecycle. In some cases, you might also want to have the application context available in the graph. For that, you would also need the graph to be in the application class. once advantage of this  approoach is that the graph is available to other Android framework classes. Additionally, it simplifies testing by allowing you to use a custom application class in tests. 

Becuse the interface that generates the graph is annotated with @Component, you can call it Application Component or ApplicationGraph. You usually keep an instance of that component in your custom application class and call it every time you eed the application.

```
// Definition of the application graph
@Component
interface ApplicationComponent { ... }

// aaComponent  lives in the application class to share its lifecycle
class MyApplicatiion: Application() {
  // Reference to the application graph that is used across the whole app
  val appComponent = DaggerApplicationComponent.create()
}

```

Becuase certain Android famework classes such as activites and fragments are instantiated by the system, Dagger can't create them for you. For activites specifically, any initialization code needs to go into the onCreate() method. That means you cannot use the @Inject annotation in the construcot of the class(constructor injection) as you did in the previous examples. Instead, you have to use field injection. 

Instead of creating the dependencies an activity requires in the onCreate() method, you want Dagger to populate those dependencies for you. For field injectionnn, you instead apply the @Inject annotation to the fields that  you want to get from the Dagger graph. 

```
class LoginActivity: Activity() {
  // You want Dagger to provide an instance of longinViewModel fromt he graph
  @Inject lateinit var loginViewModel: LoginViewModel
}
```

## Injecting activites
DAgger needs to know that LoginActivity has to access the graph in order to provide the viewmodel it requires. Use the @Componet interface to get objects from the graph by exposing functions with the return type of what you want to get from the graph. In this case, you need to tell Dagger about an object that requires a dependency ot be injected. For that, you expose a function that takes as a parameter the object that  requests injection. 

```
@Component
interface ApplicationComponent {
  // This tells dagger that LoginActivity requests injection so the graph needs to satisfy all the dependencies of the fields that LoginActivity is requesting. 
  fun inject(activity: LoginActivity)
}
```

This function tells Dagger that LoginActivity wants to access the graph and requests injection. Dagger neeeds to satisfy all the dependencies that LoginActivity requires. If you have multiple classes that request injection, you have to specifically declare them all in the conponent with their exact type. For example, if you had LoginActivity and RegistrationActivity requresting injection, you'd have two inject() methods instead of a generic one coverfing both cases. A generic inject() method doesn't tell DAgger what needs to be provide. The functions in teh interface can have any name, but calling them inejct() when they receive the object to inejct as parameter is a convention in DAgger. 

To inject an object in the activity, you'd use the appcoponent defined in your application class and call the inject9) method, passing in an instance of hte activity that request injection. 

When using activites, inject Dagger in the activity's onCreate() methods before calling super. onCreate() to avoid issues with fragment restoration. During the restore phase in super.onCreate(), an activity attaches gragments that might want to access activity bindings. 

When using fragments, inject Dagger in teh fragment's onAttach() method. In this case it can be done before or after calling super.onAttach(). 

```
class LoginActivity: Activity() {
  // You want Dagger to provide an instance of LoginViewModle from the graph. 
  @Inject lateinit var loginViewModel: LoginViewModel
  
  @Override fun onCreate(savedInstanceState: Bundle?) {
    // Make Dagger instantiate @Inject fields in LoginActivity
    (applicationContext as Myapplication).appComponent.inject(this)
    
    // Now LoginViewModel isavaiable
    super.onCreate(savedInstanceState)
  }
}


// @Inject tells Dagger how to create instance of LoginViewModel
class LoginViewModel@Inject constructor( 
  private val userREpository: UserRepository
) { ... } 
```

Provide additional depdencies to the graph. 
```
class UserRepository @Inject constructor (
  private val localDataSource : UserLocalDataSource,
  private val remoteDataSource: UserRemoteDataSource
) { ... }
```

## Dagger Modules
Apart from the @Inject annotation, there's another way to tell Dagger how to provide an instance of a class: the information inside Dagger modules. A Dagger module is a class that is annotated with @Module. There, you can define dependencies with the @Provides annotation

```
// @Module informs Dagger that this class is a Dagger Module
@Module 
class NetworkModule {
  // @Provides tell Dagger how to create instances of the type that ithis function returns i.e. LoginRetrofitService).
  // Function parameters are the dependencies of this type. 
  @Provides provideLoginRetrofitService(): Login RetrofitService {
    // Whenever Dagger needs to provide an instance of type LoginRetrofitSErvice, this code is run
    return Retrofit.Builder()
      .baseUrl("https://example.com")
      .build()
      .create(LoginService::class.java)
  }
}
```

Modules are a way to semantically encapsulate information on how to provide objects. As you can see, you called the class NetworkModule to group the logic of providing objects related to networkign. If the application expands, you can also add how to provide an OkHttpClient here, or how to configure Gson or Moshi

The dependencies of a @Provides methods are the parameters of that method. For the previous method, LoginRetrofitService can be provide dwith no dependencies becuase the method has no parameters. If you had declared an OkHttpClient as a parameter, Dagger would need to provide an OkHttpClient instance from the graph to satisfy the dependencies of LoginRetrofitService. 

```
@Module
class NetworkModule {

  // Hypothetical dependency on LoginRetroFitService
  @Provides
  fun provideLoginRetrofitService(
    okHttpClient: OkHttpClient
  ): LoginRetrofitService { ... }
}
```

In order for the Dagger graph to know about this module, you have to add it the @Component interface as follows
```
// The "modules" attribute in teh @Component annotation tells Dagger that Modules to include when building the graph
@Conponent(modules = [NetworkModule::class])
interface ApplicationComponent {
  ...
}
The recommended way to add types to the Dagger Graph is by using constructor inejction. Sometimes this is not possible and you have to use Dagger modules. one example is when you want Dagger to use the reulst of computation to determine how to create an instance of an object. Whenever it has to provide an instance of that type, Dgger runs the code inside the @Provides method

```

## Dagger scopes
Becuase you might want to use UserRepository in other features of the app and might not want to create a new object every time you need, it, you can designate it as a unique instance for the whole app. It is the same for LoginRetrofitService: it can be expensive to create, and you also want a unique instance of that object to be reused. Creating an instance of UserRemoteDataSource is not that expensive, so scoping it to the conponents's lifecycle is not necessary. 

@SingleTone is the only scope annotation that comes with the javax.inject package. you can use it to annotate ApplicationComponent and the objects you want to reuse across the whole application. 

```
@Singleton
@Component(modules = [NetworkModule::class])
interface ApplicationComponent {
    fun inject(activity: LoginActivity)
}

@Singleton
class UserRepository @Inject constructor (
   private val localDataSource: UserLocalDataSource
   private val remoteDataSource: UserRemoteDataSource
) { ... }

@Module
class NetworkModule {
    // Way to scope types inside a Dagger Module. 
    @Singleton
    @Provides
    fun provideLoginRetrofitService(): LoginRetrofitService { ... }
}
```
Take care not to inroduce memory leaks when applying scopes to objects. As long as the scoped component is in memory, the created object is in memory too. Becuase ApplicationComponent is created when the app is launched (in the application class), it is destoryed when the app  gets destroyed. Thus, the unique instance of UserREpsotiory always remains in memory until the application is destroyed. 


## Dagger Subcomponents
If your login flow (managed by a single LoginActivity) consists of multiple fragments, you should reuse the same instance of LogingViewModel in all fragments. @Singleton cannot annotate LoginViewModel to reuse the instance for the following reasons 
1. The instance of LoginViewModel would persist in memory after the flow has finished. 
2. You want a different instance of LoginViewModel for each login flow. For example if the user logs out, you want a different instance of LginViewModel, rather than the same instance as when the user logged in for the first time. 

To scope LoginViewModle to the lifecycle of LoginActivity you need to create a new component ( a new subgraph) for the login flow and a new scope. 

```
@Component
interface LoginComponent {}
```

Now, LoginActivity should get injections from LoginComponent becuase it has a login-specific configuration. This ermoves the responsibility to inject LoginActivity from the Applciation Component class. 

```
@Component
interface LoignComponent( {
    fun inject(activity: LoginActivity)
}
```

LoginComponent  must be able to access the objects form ApplicationComponent becuase LoginViewModel depends on UserRepository. The way to tell Dagger that you want a component to use part of another component is with Dagger subcomponents. The new component must be a subcomponent of the component containing shared resources. 

Subcomponents are components that inherit and extend the object graph of a parent component. Thus, all objects provided in the parent component are provided in the subcomponent too. In this way, an object form a subcomponent can depend on a object provied by the parent component. 

To create instances of subcompnents, you need an instance of the parent  compnent. Therefore, the objects provided by the parent compnent to the subcompnent are still scoped to the parent component. 

```
// @Subcomponent annotation infroms Dagger this interface is a Dagger Subcomponent.
@Subcomponent
interface LoginComponent {
    // This tells Dagger that LoginActivity request injection from Login Compnent so that this subcompnent graph needs to satisfy all teh dependencies of the fields
    // that LoginActivity is injectin
    fun inject(loingActivity: LoginActivity) 
}
```

You also must define a subcomponent factory inside LoginComponent so that ApplicationComponent knows how to create instance of LoginComponent. 

```
@Subcomponent
interface LoginComponent {
    // Factory that is used to create instance of this subcomponent
    @Subcomponent.Factory {
        fun create(): LoginComponent
    }
    
    fun inject(loingActivity: LoginActivity)
}
```

To tell Dagger that LoginComponent is a subcompnent of ApplicationComponent, you have to indicate it by 

1. Creating a new Dagger module passing the subcomponents class to the subcomponents attribute of thea nnotation
```
// The subcomponents attribute in the @Module annotation tells Dagger what
// Subcomponents are children of the Component this module is included in. 
@Module(subcomponents  = LoginComponent:class)
class SubcomponentsModule {}
```
2. Adding the new module

```
// Including SubcomponentsModule, tell ApplicationComponent that
// LoginComponent is its cusbcomponent. 
@Singleton
@Component(modules = [NetworkModulle::class, SubcomponentsModule::class])
interface ApplicationComponent
```
Note that ApplicationComponent doesn't need to inject LoginActivity anymore becuase that responsibility now belongs to LoginComponent, so you can remove theinejct() method from ApplicationComponet.

Consumers of ApplicationComponent need to know how to create instances of LoignComponent. THe parent component must add a method in its interface to let consumers create instances of teh subcomponent out of an instance of the parent compnent. 

3. Expose the factore that creates instances of Loign COmponets in the interface. 
```
@Singleton
@Component(modules = [NetworkModules::class, Subcomponents::class]) 
interface ApplicationComponent {
    // This function exposes the LoginComponent factory out of the graph so consumers 
    // can use it to obtain new instances of LoginComponent. 
    fun loginComponent(): LoginComponet.Factory
}
```
5.
