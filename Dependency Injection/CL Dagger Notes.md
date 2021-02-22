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
