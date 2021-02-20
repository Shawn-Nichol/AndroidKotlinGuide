Android recommended app architecture encourgaes dividing your code into classes to benefit from separation of concerns, a principle where each class of the hierarchy has a single defined responsibility. This leads to more, smaller class that need to be connected together to fulfill each other's dependencies. 

The dependencies between classes can be represented as a graph, in which each class is connected to the classes it depeneds on. The representation of all your classes and their dependencies makes up the application graph.

Dependency inejction helps make these connections and enables you to swap out implementations for testing. For exmaple, when testing a ViewModel that depends on a repository, you can pass different implementations Repsotiry with either fakes or mocks to test the differenct cases. 

## Basics of manual dependency injection
Consdider a flow to be a group of screens in your app that conrresponds to a feature. Login, registration, and checkout are all examples of flows. 

When covering a login flow for a typical Android app, the LoginActivity depends on LoginViewModel, which in turn depeneds on UserRespository. Then UserRepostiroy depends on a UserLocalDatasource and UserRemoteDataSource, which in turn depends on a Retrofit service.

LoginActivity is the entry point to the login flow and the user intereacts with the activity. Thus LoginActivity needs to create the LoginViewModel with all its dependencies. 

The repository and  DataSource classes of the flow look like this. 
```
class UserRepository(
  private val localDataSource: UserLocalDataSource,
  private val remoteDataSource: UserRemoteDataSource
) { ... }

class UserLocalDataSource { ... }
class UserRemoteDataSource (
  private val loginService: LoginRetrofitService
) { ... }
```

```
class LoginActivity: Activity() {

  private lateinit var loginViewModel: LoginViewModel
  
  override fun onCreate(...) {
    super.onCreate(savedInstanceState)
    
    // In order to satisfy the dependencies of LoginViewModel, you have to also satisfy the dependencies of all of its dependencies recursively. 
    
    // First, create retrofit wich is the dependency of UserRemoteDataSource
    val retrofit = Retrofit.Builder()
    .baseUrl("https:example.com")
    .build()
    .create(LoginSErvice:class.java)
    
    // Then, satisfy the dependencies of UserRepositroy
    val remoteDataSource = UserRemoteDataSource(retofit)
    val localDataSource = UserLocalDataSource()
    
    // Now you can create an instance of UserRepository
    val userRespository = UserRepository(localDataSource, remoteDataSource)
    
    // Last, create an instance of LoginViewModel with userRepository
    loginViewModel = LoginViewModel(userRepository)
  }
}
```

There are issues with this approach
1. There's a lot boilerplat code. If you wanted to create another instance of LoginViewModel in another part of the ccode, you'd have code duplication. 
2. Depeendencies have to be declared in order. You have to instantiate UserRepostory before LoginViewModel in order to create it. 
3. It's difficult to reuse objects. if you wanted to reuse UserRepostory across multiple features, you'd have to make it follow the singleton patter. The signleton pattern makes testing more difficult becuase all tests share the same singleton instance. 

## Managing dependencies with a container. 
To sovle the issue of reusing objects, you can create your own dependencies container, class that you use to get dependencies. All instances provided by this container can be public. In the exmaple, becuase you only need an instance of UserRepsotory, you can make its dependencies private with the option of making them public in the future if they need to be provieded. 

```
// Container of objects shared across the whole app
class AppContainer {
  // Since you want to expose userRepository out of the container, you need to satisfy its dependencies as you did before. 
  ```
  private val retrofit = Retrofit.Builder()
    .baseUrl("https://example.com")
    .build()
    .create(LoginService::class.java)
    
    private val remoteDataSource = userReoteDataSource(retrofit)
    private val localDataSource = UserLocalDataSource()
    
    // Userrepsotory is not private; it'll be exposed
    val userRepostory = UserRepository(localDatasource, remoteDataSource)
  ```
}
```

Becuase these dependencies are used across the whole application, they need to be placed in a common place all activites can use: the application class. Create a custom application class that contains an AppContainer instance. 

```
// Custom Application class that needs to be specified in the Android Manifest.xml file
class  MyApplication: Application() {
  // Instance of AppContainer that will be used by all the activites of the app
  val appContainer = AppContainer()
}
```

Now you can get instance of the AppContainer from the application and obtain the shared of UserRepository. 
```
class LoginActivity: Activity() {
  private lateinit var loginViewModel: LoginViewModel
  
  override fun onCreate(...) {
    super.onCreate(savedInstancesState)
    
    // Gets userRepository from the instance of AppContainer in Application
    val appcontainer = (application as MyApplication).appContainer
    loginViewModel = LoginViewModel(appContainer.userRepository)
  }
}
```

In this way, you don't have a singleton UserRepository. Instead, you have an AppContainer shared across all activites that contains objects from the graph and creates instances of those objects that other classes can consume. 

If LoginViewModel is needed in more places in the application, having a centralized place where you create instances of LoginViewModel makes sense. You can move the createion of LoginViewModel to the container and provide new objects of that type with a factory. The code for aLoginViewModelFacotry looks like this. 

```
// Definitino of a Factory interface with a function to create objects of a type. 

inteface Factory<T> {
  fun creat(): T
} 

// Factory for LoginViewModel, Since LgoinViewModel dependes on UserRepository, in order to create instances of LoginViewModel, you need an instance of UserRepository that you pass as a parameter. 
class LoginViewModelFacotry(private val userRepository: UserRepository) : Factory {
  override fun create(): LoginViewModel {
    return LoginViewModel(userRepository)
  }
}
```

You can include the LogoinViewModelFactory in the AppContainer and make the LoginActivity consume it. 
```
// AppContainer can now provide instances of LoginViewModel with LoginViewModelFactory
class AppContainer {
  ...
  val userRepsotory = UserRepository(localDataSource, remoteDataSource)
  
  val loginViewModelFactory = LoginViewModelFactory(userRepository)
}


class LoginActivit: Activity() {
  private lateint var loginViewModel: LoginViewModel
  
  override fun onCreate(...) {
    super.onCreate(savedInstanceState)
    
    // Gets LoginviewModelFactory from the application instance of AppContainer to create a new LogiViewModel instance. 
    val appContainer = (application as MyApplication).appContainer
    loginViewModel = appContainer.loginViewModelFactory.create()
  }
}
```

This approach is better than the manual inejction, but there are still some challenges to consider
1. You manage the AppContainer yourself, creating instances for all dependencies by hand. 
2. There is still a lot of boilerplate code. You need to create factories or parameters by hand depending on whether you want to reuse an object or not. 

## Managing dependencies in application flow
AppContainer gets complicated when you want to include more functionality in the progject. When your app becomes larger and you start introducing different feature flows, there are even more probelms that can occur.

1.  When you have different flows, you might want object to just live in the scope of that flow. Fore example, when creating LoginUserData(that might consist of the username and password used only in the login flow) you don't want to persist data from an old login flow from a different user. You want a new instance for ever new flow. YOu can achieve that be creating FlowContainer objects inside the AppContainer. 

2. Optimizing the application graph and flow container can also be difficult. You need to remeber to delete instances that you don't need, depending on the flow you're in. 

Imagine you have a login flow that consists of one activity and multiple fragments

```
class LoginContainer(val userRepository: UserRepository) {
  val loginData = LoginUserData()
  
  val loginViewModelFactory = LoginViewModelFactory(userRepository)
}

// AppContainer contains LoginContainer now

class AppContainer {
  ...
  val userRepository = UserRepository(localDataSource, remoteDataSource)
  
  // LoginContainer will be null when the user is not in the login flow
  var loginContainer: LoginContainer? = null
```

Once you have a container specific to a flow, you have to decide when to create and delete the container instance. Becuse your login flow is self-contained in an activity,(LoginActivity), the activity is the one managing the lifecycle of the container. LoginActivity can create the instance in onCreate(0 and delte it in onDestory()

```
class LoginActivity: Activity() {
  private lateintevar loginViewModel: LoginViewModel
  private lateinit var loginData: LoginUserData
  private lateinit var appContainer: AppContainer
  
  override fun onCreate(savedInstanceSTate: Bundle?) {
    super.onCreate(savedInstanceState)
    appContinaer = (application as MyApplication).appContainer
    
    // Login flow has started. Populate loginContainer in AppContainer
    appContainer.loginContainer = Logincontainer(appContainer.userRepsotory)
    
    loginViewModel = appContainer.loginContainer.loginViewModelFactory.create()
    loginData = appContainer.lgoinContainer.loginData
  }
  
  override fun onDestory() {
    // Login flow is finishing
    // Removing the instance of login"Container in teh AppContainer
    appContainer.loginContainer = null
    super.onDestroy()
  }
}
```

Like LoginActivity, login fragmetns can access the LoginContainer from AppContainer and use the shared LoginUserData instance. Becuase in this case you're dealing with view lifecycle logic, using lifecycle observation makes sense. 


## Conclusion
Dependency injection is a good technique for creating scalable and tesable Android apps. Use containers as a way to share instance of classes in different parts of your app and as a centralized place to create instances of classes using factories. 

When your application gets larger, you will start seeing that you write a lot of boilerplate code (such as factoreis), which can be error-prone. You also have to manage the scope and lifecycle of the containers yourself, optimizing and discarding containers that are no longer needed in order to free up memory. Doing this incorrectly can lead to sbtle bugs and memory leaks. 

