`Inversion of Control`  principle in which generic doe controls the execution of specific code. 

## Annotation
`@Inject` Use injection to add types to the Dagger graph whenever it's possible. When not </br>
`@Binds` to tell Dagger which implementation an interface should have. </br>
`@Binds Instance` For objects that are constructed outside of the graph. 
`@Component` interface gives the infromation Dagger needs to generate the graph at copmile time. The parameter of the interface method define what classes request injection. 
`Module` are a way ot encapsulate how to provide objects in a semantic way. 
`@Provides` to tell Dagger how to provide classes that your project doesn't own. </br>
`@Sinlgeton` scope only allows the component to be created once, all of the appcomponents will only be created once. 
`@Subcomponent` are components that inherit and extend the object graph of a parent component. Thus, all objects provided in the parent component will be provided in the subcomponent too. in this way, an object form a subcomponent can depend on an object provided by the parent compoent. 


## Dagger Graph
The implementation of the application graph is automitcally genertated by the annotation processor. The generated class is called Dagger{ComponentName}
