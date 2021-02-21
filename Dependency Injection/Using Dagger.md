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
