Dependency Injection is a technique widely used in programming and well suited to android development. By following the principles of DI, you lay the groundwork for  good app architecture. Implementing depenecy injectino provides the following advantages. 
- Reusability of code. 
- Ease of refactoring
- Ease of testing

# Fundamentals of DI
Class often require reference to other classes. Fore example, a car class might need a reference to an engine class. These reqquired classes are called dependencies, and in this example the car class is dependent on having an instance of the engine class to run. 

There are three ways for a class to get an object it needs. 
1. The class constructs the dependency it needs. In the example above, Car would create and initialize its own instance of engine. 
2. Grab it from somewhere else. Some Android APIs, such as context getters and getSystemService(), work this way. 
3. Have it uspplied as a parameter. The app can provide these dependencies when the class is constructoed or pass them in the functions that need each dependency, the car constructor would receive engine as a aparametere. 

The third option is dependency injectino! with this appraoch you take the dependencies of a class and provide them rather than having the class instance obtain them itself. 

```
class Car {
  private val engine = Engine()
  
  fun start() {
    engine.start()
  }
  
  fun main(args: Array) {
    val car = Car()
    car.start()
  }
}
```

This is not an examlple of dependency injection becuase the car class is constructing its own engine. This can be problematic becuase  
- Car and Engine are tightlly coupled - an instance of Car uses on type of Engine, and  no subclasses or alertnative implementations can easily be used. If the car were to construct its own Engine, you would have to create two teypes of Car instead of just reusing the same Car for engines of types Gas and Electric. 
- The hard dependency of Engine makes testing more difficult. Car uses a real instance of Engine, thus preventin yo ufrom using a test double ot modify Engine for differenct test cases. 

Instead of each instance of Car constructing its own Engine object or initialization, it receives an Engine object as a parameter in its constructor. 

````
Class Car(private val engine: Engine) {
  fun start() {
    engine.start()
  }
  
  fun main(args: Array) {
    val engine = Engine()
    val car = Car(engine)
    car.start()
  }
}
```

The main functino uses Car. Becuase Car depends on Engine, the app creates an instance of eEngine and then uses it to construct an instance of Car. The benefits of this DI-based apprach are

- Reusability of Car. You can pass in different implementations of Engine to Car. For example you might define a new subclass of Engine called ElectricEngine that you want Car to use. If you use DI, all you need to do is pass in an instance of the updated ElectricEngine subclass, and Car still works without any further changes. 
- Easy testing of Car. You can pass in test doubles to test your different scenarios. For example, you might create a test double of Engine called FakeEngine and configure it for different tests. 

There are two major wasy to do DI in android 
- Cosntructor injection .This is the way described above. You pass the dependencies of a class to its constructor. 
- Field Injection(or Setter Injection). Certain Android framework classes such as activies and fragments are instantiated by the system, so constructor injection is not possible. With field injection, dependencies are instantiated after the class is created. The code would like this. 

```
class Car {
  lateinit var engine: Engine
  
  fun start() {
    engine.start()
  }
}
  
fun main(args: Array) {
  val car = Car()
  car.engine = Engine()
  car.start()
}
```

## Automated dependency Injection
The previous exmaples are refereed to dependency injection by hand, or manual dependency injection.

- For big apps, taking al l the dependencies and connectin them correctly can require a large amount of boilerplate code. In a multi-layered architecture, in order to create an object for a top layer, you have to provide all the depenedcies of the layers below it. As a concrete example, to build a real car you migh need an engine a transmission, a chassis and other parts; and an engine in turn needs cylinders and spark plugs. 

- When you're nota ble to construct dependencies before passing them in - for examole when using lazy insinitlizatino or scoping object to flow of your app you need to write and maintain a custom container or graph of dependencies that managges the lifetimes of your dpenedencies in memory. 

THere are libraires that solve this probelm by automatin th e process of creating an dprovidng dependencies. They fit into tow categories. 
- Reflectino-bases solutions that connect dependencies at runtime. 
- Static solutions that generate the codes to connect depenedenceis at compile time. 

Dagger is a popular dependency injection library for Java, Kotlin and Android that is maintaned by Google. Dagger facilitates uisn DI in your app by creating and managinthe graph aof dependencies for you. It provides fully static and compile-time dependeencies addressing many o fthe development and perfromance issues of relection-bases solutions such as Guice. 

## Alternatives to dependency injection
An alternative to dependency injectin is using a service locator. The service locator design patter aslo improves decoup0ling of classes from concrete dependencies. You createa  classsknown as the service locator that creates an stores dependencies and then provide s those dependencies on demand. 

## Alertantives to dependency injection
An alternative to dependency injection is using a service locator. The service locator design patter also improves decoupling of classses from concrete dependencies. You create a a class known as the service locator that createws and stores dependencies and then provides those dependencies on demand. 

```
object ServiceLocator {
  fun getEngine(): Engine = Engine()
}

class Car {
  private val engine = ServiceLocator.getEngine()
  
  fun start() {
    engine.start()
  } 
}

fun main(args: Array) {
  val car = Car()
  car.start()
}
```

The service locator pattern is different from dependency injection in the way the elements are consumed. With the service locator pattern, classes have control and ask for objects to be injected; with dependency injection, the app has control and proactively inejcts the required objects. 

Compared to dependency injection
- The collection of dpenedencies require by a service locatore makes code hard to test becuase all the tests have to inertact with the same global service locator. 

- Dependencies are encoded in the class implementation, not in the API surface. As a result, it's hrader to know what a class needs from the outside. As a result, changes to Car or the dependencies available in eh service locator might result in runtime or test failures by causing reference to fail. 

- Managin liftimes of objects is more difficult if you want to scope to anything other than the lifetime of the entier app. 

## Use hilt in your android app. 
Hilt is Jetpack's recommended library for dependency inejction in Android. Hilt defines a standard way ot do DI in your application by provding containers fore every Androdi class in your project and managing theier lifecycles automatically for you. 

Hilt is built on top of the populat container fo ever android class in your project and managin their lifecycles automatically for you . 

Hilt is build on top of the popular DI library Dagger to benefit form the compile time correctness, runtime perfromacne scalabilit, and Androi dstuido support that dagger provides. 

## Conclusion
Dependency injectin provides your app with the following advantages. 
- Reusability of classes and decoupleing of dependencies: It's easier to swap out implementations of dependency Code reuse is improved becuase of inversion of control, and no longer control how theier dependencies are created, but instead work with any configuration. 
-  Ease of refactoring: The dependencies beocme a verifiable part of the API surface, so they can be checked at object-creation time or at compile time rather than being hidden as implementations detials. 
-  Ease of testing: A class doesn't manage its dependencies, so when you're testing it, you can pass in different implementations to test all of your different cases. 
