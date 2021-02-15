# Key Points
- Unit tests verify how isolated parts of your code work. 
- Every test has three phases, set up, assertion and teardown. 
- Using Junit, you can write unit tests asserting results, meaning, you can compare an expected result with the actual one. 
- In TDD, you start by writing a test,. You then write the ocde to make the tes compile. Next you see that the tes fails. Finally, you add the implementation to the methodd under test to make it pass. 

## Unit test should follow the Given When Then
Given: Setup the objects and the state of the app that you need for the test </br>
When do the acutal action on the object you're testing </br>
Then: checks what happens when you do the action where you check if the tes passed or failed. This isuaully a number of assert function calls. </br>

## AndroidX Test
AndroidX test is a collection of libraries for testing. It includes classes and methods that give you version sof components like applications and Activites, that are meant for tests. 

Get context
```
ApplicatoinProvider.getApplicationContext()
```

AndroidX test works in both Unit test and Instruement test. 
