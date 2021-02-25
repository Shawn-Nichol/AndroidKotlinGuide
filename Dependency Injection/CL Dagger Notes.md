To apply an annotation to the constructor, you need to specifically add the keyword constructor and introduce the annotation just before it. 
```
class RegistrationViewModel @Inject constructor( val userManager: UserManager)  { ... }
```

With the `@Inject` annotation, Dagger knows
1. How to create instnces of type RegistrationViewModel
2. RegistrationViewModel has UserManager as dependency since the constructor takes an instance of UserManager as an argument. 


## View requireed objects
Certain Android famework classes such as Activites and Fragmetns are instantiated by the system so Dagger can't create them for you. For activites any initialization code needs to go to the onCreate method. Becuase of that, we cannot use th @Inject annotation in the constructor of a View class as we did before that is what is called constructor injection. instead we have to use filed inject. 

Instead of creating the depndencies an Activity requires in the onCreate method as we do with manual depndency injection, we want Dagger to populate those dependencies for us. For field injection (that is commonly used in Activites and Fragments), we annotate with @Inject the fields that we want Dagger to provide. 

@Componet interface gives the information Dagger needsd to generate the gtrpah at compile-time. The parameter of the interface mthods define what classes request injection. 


`@Proivde` use Provides to tell DAgger how to provide a class that your probject doesn't own.

Modules are way to encapsulate how to provide objects in semantic way.

`@BindsInstance` for objects that are constructed outside of the graph (eg, context)


Abstract class cann not instantiated means we can not create object for that class
- We can't create an object from an abstract class
- All the variables and member functions of an abstract class are by default non-abstract. So, if we want to override these members in the child class then we nneed to use open keyword.
- If we delcare a member function as abasstract then we do not need to annotate with open keyword.
- An abstracdt memeber function doesn't have a body, and it must be implemented in the derived class

`Lazy` the value gets computed on first use. 


An Activity injects Dagger in the onCreate method before calling super. 
A Fragment injects Dagger in the onAttach method after calling super. 

There are two ways to interact with the Dagger graph. 
1. Declaring a function that returns unit and  takes a class as a parametere allows field injection in that class
2. Declaring a function that returns a type allows retrieving types from the graph. 

## Scoping Rules
When a type is marked with a sscope annotations, it can only be used by components that are annotated with the same scope
- When a Component is marked with a scope annotation, it can only provide types with that annotation or types that have no annotation. 
- A subcomponent cannot use a scope annotation used by one of its paretn components. 
- 
