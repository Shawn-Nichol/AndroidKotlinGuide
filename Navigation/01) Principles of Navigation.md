# Principles of Navigation
Navigation between different screens and apps is a core part of the user experience. The following principles sets a baseline for a consistent and intuitive user experience across apps. The Navigation component is desnged to implement these principles by default, ensuring that users can apply the same heuristics and patterns in navigation as they move between apps. 

## Fixes start destination
Every app you build has a fixed start destination. This is the first screen the user sees when they launch your app from the launcher. This destination is also the last screen the user seees when they return to the launcher after pressing the back button.  

## Navigation state is represented as a stack of destinations
When your app is first launched, a new task is created for the user, and app displays its stsart destination. This beocmes the base destination of what is knowwn as the back stack and is the basis for your app's navigation state. The top of the stack is the current screen, and the previous destinations in the stack represent the history of where you've been. The back stack always has the start destnination of the app at the  bottom of the stack. 

Operations that change the back stack always operate on the top of the stack, either by pusing a new destination onto the top of the stack or popping the top-most destnation off the stack. Navigating to a destnation pushes that destination on top of the stack. 

The Navigation component manages all of your back stack ordering for you, though you can also chooose to manage the back stack yourself. 

## up and back are identical within your app
The back button appears in the system navigation bar at the bottom of the screen and is used to navigate in reverse-chronological order through the history of screens the user has recently worked with. When you press the Back button, the current destination is popped off the top of the back stack and you then navigate to the previous destination. The back button will exit the app when pressed at the app's start destination. 

The up button appears in the app bar at the top of the screen. The Up buttons behaves identaiclly to the back button, unless the user is at the app's start destination, then the up button does not appear, becuase the UP button never exits the app.. 

When your app is launched using a deep link on another app's task, Up transistions user back to your app's task and through a simulated back stack and not to the app that triggered the deep link. The back button, however, does take you back to the other app. 

## Deep linking simulates manual navigation

Whether dep linking or manually navigating to a specific destination, you can use the up button to navigate through destinations back to the start destination. 
When deep linking to a destination within your app's task, any existing back stack for your app's task is removed and replaed with the deep-linked back stack. 
