## Blackbox Testing
- Configure what you're testing.
- Execute the method that ou want to test
- Finally verify the result by chcking the state of the object under test. 

`State verification`: The object under test has to collaborate with another one. Becuase you want to focus on the first object, in the configuration phase, ou want to provide a test double collaborator to your object under test. The fake collaborator is just for testing purposes and you configure it to behave as you want. 

`Stubbig method`: set the method to always return the same result. 

## Whitebox testing
You want to ensure that hyour object under test will call specific collaborator methods. ex you may have a repository object that retrieves the data from the newtork, and before returning the results, it calls a collaborator object to save them into a database. You can use Mockito to keep an eye on a collaborator and verify if sspecific methods were called on it. 


## Setting up Mockito
add dependencies 
```
dependencies {
  testImplementation 'com.nhaaramn.mockitokotlin2:mockito-kotlin:2.1.0'
}
```
Mockito-Kotlin is a wrapper library around Mockito. It provides top-level functiosn to allow for a more kotlin like approach and also solves a few issues with using the Mockito Java library in kotlin. 

Unit tets
