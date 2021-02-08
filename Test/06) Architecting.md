# Key points
- Use Software architecture to communicate development standards between team memebers.
- It's not uncommon to reusee software architecture on different projects
- When starting a new progject, on e of the first decisions is to decide on its software architecture.
- Proper software architecture helps with testing
- Support your software architecture using design pattters and the SOLID principles
- Design pattersn are classified into thredd categories: creational, structural and behavioral
- The dependency injection pattern i the key to having a testablel architecutre
- There are other user interfacce design pattersn such as MVC, MVP and MVVM. 

## Architecting
- Use software architecture to communicate development standards between team members
- It's not uncommon to reuse software architecture on different projects
- When starting a new project, one of the first decions is to decide on its software architecture
- Proper software architecture helps with testing.

## S.O.L.I.D principles
`SRP`: Single Responsbility, each class should have a unique objective of should be useful for a specific case. Any lgoic that is not part of the objective of the class should be the responsibility of some other class. A Class that has lots of responsibilities is sometimes called a god class. </br>

`Open-Closed`: Software entities of your app: classes, methods, etc,. Should be open for extension but closed for  modification. This means that you should desing them in such a way that adding new features or modifying behavior shouldn't require you to modify your exisitn code but instead add new code or creat new classes. 

`Liskov substituion`: It states that n app taht uses an object of base class should be able to use objects of derived classes without knowing about that and continueworking. Therefore, your code should notbe  checkign the subtype. In the subclass you can override some of the parent methods as long as you continue to comply with its semantics and maintain the exepcted behaivior. </br>

`Interface segreation`: This principle encourages you to create fine grained interface that are client specific. Suppose ou have a class with a few methods, one part of your app may only need to accessa subset of your methods and other part may need ot acces another subset. This primciple encourrages you to create tow interfaces. Clients hould have access to only what they need and nothing more. </br>

`Dependency inversion`: THis principle states that a concrete class A shouuld not depend on a concrete class B but an abstaction of B instead. This abstraction could be an interface onan abstact class. 

##  Designe patterns are classified into three categories:, 
  `creational`: The patterns in the creational category describe solutions related to object creation. 
  `structural`: design patterns ease the design to establish relationships between objects.
  `Behavioral`: Behavioral design patterns explain how ojbects interact and how a task can be divided into sub-tasks among different objects. While creational pattersn explain a sepcific momen tof time and structural pattersn descibe a static structure, behavioral patterns descibe a dynamid flow. 

## Interface desing Patterns 
### MVC
`Model`: The data classes that model your business belong to this layer. They usually contain data and buiness logic. This layer also contains the classes that fetch and create objects of those business classes, networking, caching and handling databases. </br>
`View`: This layer displays data from the Model. it doesn't contain any business logic. 
`Controller`: The goal of this layer is to connect the objects of the Model and VIew layers. It gets notfied of user interaction and updates the Model. it retrieves data from the Model. It may also update the View when there's a change in the Model. 

### MVP 
`Model`: The data classes that model your business belong to this layer. they usually contain data nd buiness lgoic. This layer also contains the classes that fetch and create objects of those business classes, networking caching and handling databases. </br>
`View`: DIsplays data presented by the presenter but doesnt' have a reference to the Model. it does, however, have a reference to the presenter to notify it about user actiosn. 
`Presenter`: The Presnter retrieves data form the Model and updates it accordingly. It has UI presentation logic that decides twhat to dispaly . It notifies the View when a model has chagned. Therefore, it has a reference the View and the Model. 

### MVVM
`Model`: Same as the Model layer of MVC/MVP </br>
`View`: Notifies the ViewModel about user actions. Subscibes to streams of data exposed by the ViewModel. </br>
`ViewModel`: Retrieves data from teh Model and updates it accordingly. Exposes streams of data ready to gbe displayed, but it doesn't know and doesn't care about who is subscibed to the streams. 


`Communciation` Softawre architecture establishes a common language between the developers of an app and other members of the team, like managers, QA testers, analystand designers. </br>

`Reusable abastraction` Reusability saves time, you can reuse patterns in different parts of an app, across different apps as well as on other platforms. You'll also see that you can use architecture patterns to kick-off new projects. </br>

`Early design decisions`: When ou create a new app, one of the first decisions is to decide on the architecture you're going to use. These early design decisions are important becuase they'll set constraints on your implementattion, such as the way your classes will interact and their responsibilities. EArly decisions will also organize your codebase a specfic way. 

`Better testing`: By using good architecture fromthe start or refactoring an existing one, you'll enable the creation of tests that would wotherwise be impossible or difficult to write. Also migrating from an existin garchitecture to a better one which is a difficult task, but not impossible will enable you to migrate slower tests, such as UI or integration tests, to unit tests, which are faster. 


