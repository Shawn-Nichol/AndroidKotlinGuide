# Factory
In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the probelm of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method either specified in an interface and implemented by child classes, or implemented in a base class and optionally overrideen by derived classes rather than by calling a constructor. 

The Factory method design patter is one of the Gang of Four design patterns that descibe hos to solve recurring desing problems to design flexible and reusable object-oritented software, that is objectst that are easier to implement, change, test and reuse. 

The factory method design pattern is used instead of the regualr class constructor for keeping with in the SOLID principles of programming, decoupling the construction of objects from the objects themselves. This has the following advantages and is useful for the following cases, among others. 
- Allows construction of classes with a cmoponet of a type that has not been predetermined, but only defined in an interfface, or which isdefined as dynami type. 
- Ex, a class Vehicle that has a member motor of interafce IMotor, but no concrete type of Motor define din advance, can be constructed by telling the vehicle constructor to use an ElectricMotor or a Gasolinemotor. The Vehcile constructor code then calls a motor factory method, to create the desired Motor that compiles with the IMotor interface. 
- Allows constuction of subclasses to a parent whose componet type has not been predetermined, but only definedin an interface, or which is defined as dynaic type. 

Creating an object directly wwithin the class that requires or uses the object is inflexible becuase it commits the class to a particular object and makes it impossible to change the instantiation independetly of the class. A change to the instantiator would require a  change to the class code which we would rather not touch. This is referened to as code coup0ling and the Factory method pattern assists in decoupling the code. 

The factory Method design pattern is used by first defining a separate operation a factory method for creating an object, and then using this factory method by calling to create the object. This enables writing of subclasses that decide how a parent object is created aand what type of objects the parentn contains. 
