### What is a Broadcast?
Broadcasts are a system-wide event. 


### What is the difference between a Serializable and Parcelable?
Both are use to serialize classes. Parcelable however is optimized for andoid and should be prefered. 

### What is the difference between WorkManger and a Service?
Tasks scheduled with WorkManager are guaranteed to survicve process death. Other than that, it's is similar to IntentService.

### Name two ways, tow different apps can interact with each other? 
- intents
- Service. 

### What is a foreground service?
- A foreground has a notification well it is running and is safe from being killed by the system. 

### What happens in onResume()?
The UI enters the foreground. 

### What is ADB?
The Android Debug Bridge is a tool that is used to communicate with the android device. The IDE uses it to send commands to your device or emulator. 

### What is the difference between local unit test and an instrument tests? 
LocalUnit: runs on a JVM
Instrument unit Test: runs on an android device. 

