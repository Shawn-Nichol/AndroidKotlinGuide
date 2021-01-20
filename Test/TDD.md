# Tedst Driven Development
TDD is a process in which you write the tets for the code you are going to add or modify before you write the acutal code. Becuase it's a process and not a library, you can apply it to any project. 

## Benifts to TDD
`Write Intentinally`: Well written tests provide a description of what your code should do. From the start, you will focus on the end result. Writing these specifications as tests can keep the result from deviating from the initial idea. 

`Automatically docuemnt`: You can look at test to help understand what the code does. 

`Keep maintanable code`: TDD encourages you to pay attention to the structure of the code. Encouraging you to build your app in a testable way. 

`Have Conficdence in your code`: Tests help ensure that the code works the way it should. This allows you ensure the code still works after future changes. 

`Develop Faster`: using a process that promotes more readable and maintainable code and that acts as self documentation means you can spend less time trying to understand what the code does when revisting it, and use that time for solving your problem instead. Code you write using TDD is less error prone. 

`Higher Test coverage`: If you're writing tests alongside the code, you're going to have more test coverage over the teh code.


## Red Green Refactor
`Red`: Start any new task by writing a failing test. This is for any new feature behavior change, or bug fix that doesn't already have a test for. Run the test to make sure it fails. 

`Green`: Writ the minimum code to make the test pass. Write the needs for the tests, then run the test to see it pass. 

`Refactor`:Clean the code and makes sure the test passes. 

## Key Points
- TDD is a process of writing tests before you write your actual code. You can apply TDD to any sort of programming
- Practicing TDD helps you to code quickly and with intent, docuemtn automatically, have confidence your codei smaintainable, and ahve better test coverage
- This process should start with writing a failing test. no matter what , you always want to see the test fail. 
- only after you write your test and see it fail do you write new code or change the existing code.
- Only write enough code to  make the test pass. If there's more code you need to write, you need another test first.
