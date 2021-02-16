`TDD` Test Driven Development, write tests before writting the code. </br>
`QA` Quality Assurance </br>
`SUT` System Unit Test, is one class and you focus only on it. All dendencies are considered to be wroking correclty and ideally have their own unit tests </br>
`SRP` Single Responsibility, Each class should have a unique objective or should be useful for as specific case. Any lgoic that is not part of the objective of the class should be the responsibility of some other class. A class that has a lots ofresponsibilities is refered to as a god class.  </br>

##Blackbox testing
- Configure what you are testing
- Execute the method that you want
- Verify the result by checkign the state of the object under test. 

`State verification` The object under test has to collaborate with another one. Becuase you want to focus on the first object, in the configuration phase, you want to provide a test double collaborator to your object under test. The fake coollaborator is just for testing pruposes and you configure it to behave as you want. 

`Stubing` Set the method to always return the same result. 


## White Box testing
You want to ensur ehtat your object under test will call specific collaborator methods. ex you may have a repository object that retrieves the data from the network, and before returning the results, it calls a collaborator object to save them into a database. You can use Mockito to keep an eye on a collaborator and verify if specific methods were called on it. 


## Test Doubles
Test double is a version of a class crafted specifically for testing. It is meant to replace the real version of a clas in tests. It's similar to how a stund double is an actor who specializes in stunts, and replaces the real actor for dangerous actions. 
`Fake` A test double that has a working implementation of the class, but it's implemented in a way that makes it good for testsw but unsuitable for production. 
`Mock` Tracks which of its methods were called. it then passes or ails a test depending  on whether it's methods were called correctly.
`Stub` Includes no logic and only returns what you program it to return. A stubTaskReppository could be programmed to return certain combinations tasks. 
`Dummy` Is passed around but not used, such as if you need to provide it as a paramaeter. If you had a NoOptaskRepository, it would just implement the TaskREpository with no code in any of the  methods. 
`Spy` keeps track of some additional information; for example if you made a spyTaskRepository, i t might keep track of the number of times the addTask methodd was called.

## Anotations
`After`:The method will be executed after each test. </br>
`@AfterClass`: The method only executes once after all the tests are run. </br>
`@Before`: This method runs before each test. </br>
`@BeforeClass`: Theis method will be executed only once before all the tests are executed. </br>

## Test Runner
A test runner is a JUnit componet that runs tests. Without a test runner, your tess would not run. There's a default test runner provided by  JUnit that youget automatically. @RunWith waps out that default test runner. The AndroidJunit4 test runner allows for android X test to run your test differently depending on whether they are insturmented or locat test. 
