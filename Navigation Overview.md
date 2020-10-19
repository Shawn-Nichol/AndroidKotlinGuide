# What is Navigation
Nagivation refers to the interaction that allow users to navigate across, into and back out from the different pieces of content within your app. Android jetPack's Navigation component helps you implement navigation, from simple button slicks to more complex patterns, such as app bars and the navigation drawer. The navigation component also ensures a consistent and predicatable user experince by adhering to an established set of principles. 

The Navigation component consists of three key parts tha are descibed below. 

- Navigation graph: An XML resource that contains all navigatioon-related information in one centralized location. This includes all of the individual contents areas within your app, called destinations, as well as possible paths that a user can take through your app. 

- NavHost: An empty container that displays destinations from your navigation graph. The Navigation componetn contians a default NavHost implementations, NavHostFragment, that displays fragment destinations. 

- NavController: An Object that manages app navigation within a NavHost. The Navcontroller orchestrates the swapping of destinations content in the NavHost as users move throught your app. 

As you navigate through your app, you tell the NavController that you want to navigate either along a specific path inyour navigation graph or directly to a specific destination. The NavContoller then shows the appropriate destination in the NavHost. 

The Navigation componetn provides a number of other benefits, including the followings

- Handling fragment transactions
- handling up and back actions correctly by default. 
- Provoding standardized resources for animations and transitions. 
- Implementing and hanlding deep linking. 
-  Including Navigation UI patterns, such as navigation drawers and bottom navigation, with minimal additinoal work. 
- Safe Args- a gradle plugin that provides typ safety when navigating and passing data between destinations. 
- ViewModel support you can scope a ViewModel to a navigation graph to share UI-related data between the graph's destinations. 

In addition, you can use Android Studio's Navigation Editor to view and edit your navigation graphs. 
