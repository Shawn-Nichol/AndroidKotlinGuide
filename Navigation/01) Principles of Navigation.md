# Principles of Navigation
Navigation between different screens and apps is a core part of the user experience. The following principles sest a baseline for a consistent and intuitive user experience across apps. The Navigation component is desnged to implement these principles by default, ensuring that users can apply the same heuristics and patterns in navigation as they move between apps. 

## Fixes start destination
Every app you bild has a fixed start destination. This is the first screen the user sees when they launch your app from the launcher. THis destination is also the last screen the user seees when they reutnr to launcher after pressing the back button. Let's tak a look at the Sunflower app as an examplles. 

When launching the sunflower app from the launcher, the first screen that a user sees is th list screen, the list of plants in theier garden. This is also the last screen they see before exiting the app. If they press the back button from the list screen, they navigate back to the launcher. 

## Navigation state is represented as a stack of destinations
When your app is first launched, a new task is created for the user, and app displays its stsart destination. This beocmes the base destination of what is knowwn as the back stack and is the basis for your app's navigation state. The top of the stack is the current screen, and the previous destinations in the stack represent the history of where you've been. The back stack always has the start destnination of the app at the  bottom of the stack. 

Operations that change the back stack always operate on the top of the stack, either by pusing a new destination onto the top of the stack or popping the top-most destnation off the stack. Navigating to a destnation pushes that destination on top of the stack. 

The Navigation component manages all of your back stack ordering for your, though you can also chooose to manage the back stack yourself. 

## up and back are identical within  your app's task
The back button appears in the systme navigation bar at the bottom of the screen and is used to navigate in reverse-chronological order through the history of screens the user has recently worked with. When you press the Back button, the current destination is popped off the top of the back stack and you then navigate to the previous destination. 

The up btton appears in the app bar at the top of the screen. within your app's tasks, the Up and Back buttons behave identaiclly. 

## The up button never exits your app
If a user is at the app's start destination, then the up button does not appear, becuase the UP button never exits the app. The back button however, is shown and does exit the app. 

When your app is launched using a deep link on another app's task, Up transistions user back to your app's task and through a simulated back stack and not to the app that triggered the deep link. The back button, however, does take you back to the other app. 

## Deep linking simulates manual navigation

Whether dep linking or manually navigating to a specific destination, you can use the up button to navigate through destinations back to the start destination. 
When deep linking to a destination within your app's task, any existing back stack for your app's task is removed and replaed with the deep-linked back stack. 

Using the Sunflower app agains as a nexample, let's assume that the user had previously launched the app from the launcher screen and navigated to the detail screen for an apple. looking at the Recent screen would indicate that a task exists with th etop most screeen being the details screen for the apple. 

At this point, the user can tap the Home Button to put the app in the background. Next, let's say this happ has a deep link feature that allows users to launch directly into a specific plant detail screen by name. Opening the app via this deep liink completely replaces the current sunflower back stack shown in figure 3 with a new back stsack as shwon in figure 4. 

notice that the sunflower back stack is replaced by a synthetic back stack with the avocado detail screen at the top. The My Garden screen, which is the start destination, was also added to the back stack. This is important becuase the synthetic back stack must be realistic. It should match a back stack that could have been achieved by organically navigating through the app. The orignal Sunflower back stack is gone, including the app's knowledge that he user was on the Apple detals screen before

This navigation component support deep linking and recreates a realistic back stack for  you when linking to any destination in your navigation graph. 
