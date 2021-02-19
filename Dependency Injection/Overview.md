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
