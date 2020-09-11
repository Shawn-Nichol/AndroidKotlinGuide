# Tests
Activites serve as conteinaer for every user interaction with in your app, so it's important to test how your app's activites behave during device level events, sussh as the following. 

- Another app, such as the device's phone app, interrupts your app's activity.
- The system destroys and recreatess your activity. 
- The user places your activity in a new windowing environment, such as picture-in-picture or multi-window.

In particular, it's important to ensure that your activity behaves correctly in response to the events described in Understanding the Activity Lifecycle. 

## Drive an activity's state
