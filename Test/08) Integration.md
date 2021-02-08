# Key points
 - Integration tests verify the way different parts of your app work together. 
 - They are slower than unit tests, and should thereforre fonly be used when you need to test how things interact
 - when interacting with the Android framework you can rely on an android device or emulator or use Robolectric
 - You can use dexmaker-mockito-inline to mock final class for android tests. 
 
 
# Integration tests
When you want to createa a unit test but can't quit tests it in isolation. Integration tests tend to be slower than unit tests, but quicker than UI tets. Becuase of this, you want to first put everyting you can into unit tests. You start using integration tests when you need to test something that you cannot do without interacting with another part of your app or an external element. 
tegration test. 


- Integration tests verify the way different parts of your app work together
- Roboelectic can use dexmaker-ockito-inline to mock final classes for android test


One of the most frequent dependencies that force you to use an integration test is the android famework. This does not necessarily mean it uses the screen; it can be any component of the SDK. When your code ends up interacting with android, you can't get away with unit tests. 

You running tests that use Android framework you have two optionos
1. Run them on an emulator
2. Robelectic is a framework that allows you to run Android-dependent tests in a unit-test way. It creates a sandbox in which to run your tests; the sandbox acts like android envrionment with android APIs. A benefit to using Roboelectic is that it is faster, running on the JVM; then using a device emualtor
