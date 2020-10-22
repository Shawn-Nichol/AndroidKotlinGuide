# What is Navigation
Navigation refers to the interaction that allows users to navigate across, into and back out from the different pieces of content within your app. 

The Navigation component consists of three key parts tha are descibed below. 

1. Navigation graph
2. NavHost
3. NavController 

As you navigate through your app, you tell the NavController that you want to navigate either along a specific path in your navigation graph or directly to a specific destination. The NavContoller then shows the appropriate destination in the NavHost. 

The Navigation component provides a number of other benefits, including the followings

- Handling fragment transactions.
- Handling up and back actions correctly by default. 
- Provoding standardized resources for animations and transitions. 
- Implementing and handling deep linking. 
- Including Navigation UI patterns, such as navigation drawers and bottom navigation, with minimal additinoal work. 
- Safe Args a gradle plugin that provides typ safetye when navigating and passing data between destinations. 
- ViewModel support you can scope a ViewModel to a navigation graph to share UI-related data between the graph's destinations. 




## Glossary 

Navigation graph
An XML resource that contains all navigatioon-related information in one centralized location. This includes all of the individual contents areas within your app, called destinations, as well as possible paths that a user can take through your app. 

NavHost
An empty container that displays destinations from your navigation graph. The Navigation componetn contians a default NavHost implementations, NavHostFragment, that displays fragment destinations. 

NavController
An Object that manages app navigation within a NavHost. The Navcontroller orchestrates the swapping of destinations content in the NavHost as users move throught your app. 

NavHostFragment
NavHostFragment provides an area within your layout file for self-contained. NavHostFragment is intented to be used as the content area within a layout.

