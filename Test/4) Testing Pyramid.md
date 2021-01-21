The Testing pyramid is broken is brocken into three layers that group different types of tests and how many tests you should have in each layer. 

## Small
- Unit Test: Unit tests are the quickest, easiest to write and cheapest to run. They generally test one outcome of one meothd at a time. They are independent  of the Android framework. They are the fastest becuase they don't require a device or emulator to run.  

## Medium
- Integration Tests: Integration tests move beyond isolated elemetns aand begin testing how things work together. Thses tests are written when you need to interact with other parts of the android frame work. 

## Large Tests
UI Tests: Every android app has a UI with related testing. The tests on this layer check if the UI of the application works correctly. They usually test if the data is shown correctly ot the user, if hte UI reacts correctly whne the user inputs something. THese are the slowest and most expensive tests, becuase of the cost UI tests shoud be the least used test.


Distrbution of the tests
UI Tests 10%
Integrations Tests: 20%
Unit Tests: 70%


Key Points
-Testsing is  commonly organized into the testing pyramid.
- There are three kinds of tests in the pyramid: unit, integration, and UI, they are also called small, medium, large.
- On Android you can distinguish between local tests, which run on the JVM and instrumentations tests, which require a device or emulator. 
