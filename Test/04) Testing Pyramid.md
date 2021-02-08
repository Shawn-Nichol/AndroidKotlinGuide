# Key Points
- Testing is commonly organized into the tesing pyramid
- There are three kinds of tests in this pyramid: unit, integration, and UI other wise known as small, medium, large
- On Android you can also distinguish between local tests, whiich run on the JVM and instrumentation tests, which require a device or emulator. Ocal tests run faster than instrumented tests
- The further down on the pyrimad the more focused and more tests you'll need to write. 


The Testing pyramid is broken is brocken into three layers that group different types of tests and how many tests you should have in each layer. 

## Unit(Small) Test
Unit tests are the quickest, easiest to write and cheapest to run. They generally test one outcome of one meothd at a time. They are independent  of the Android framework. They are the fastest becuase they don't require a device or emulator to run.  

## Integration(Medium) test
Integration tests move beyond isolated elemetns aand begin testing how things work together. Thses tests are written when you need to interact with other parts of the android frame work. 

## UI (Large) Tests
Every android app has a UI with related testing. The tests on this layer check if the UI of the application works correctly. They usually test if the data is shown correctly ot the user, if hte UI reacts correctly whne the user inputs something. THese are the slowest and most expensive tests, becuase of the cost UI tests shoud be the least used test.


Distrbution of the tests
UI Tests 10%
Integrations Tests: 20%
Unit Tests: 70%



