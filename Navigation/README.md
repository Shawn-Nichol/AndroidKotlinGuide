# What is Navigation
Navigation refers to the interaction that allows users to navigate across, into and back out from the different pieces of content within your app. 

The Navigation component consists of three key parts tha are descibed below. 


`Navigation graph: ` An XML resource that contains all navigatioon-related information in one location. This includes all of the individual contents areas within your app, called destinations, as well as possible paths that a user can take through your app. 

`NavHost:` An empty container that displays destinations from your navigation graph. The Navigation component contians a default NavHost implementations, NavHostFragment, that displays fragment destinations. 

`NavController:` An Object that manages app navigation within a NavHost. The Navcontroller orchestrates the swapping of destinations content in the NavHost as users move throught your app. 




## Glossary

`Nested Graphs:` Sub flows are usually best represented as nested navigation graphs. By nesting self contained sub navigation flows this way, the main flows of your app's UI is easier to comprehend and manage. 

`NavHostFragment:` is a continaer which navigation will be using to swap in and out destinations, the NavhostFragment should be added tohe main_activity_layout.

## Destinations
`Destinations:` Are different content areas in the app, You can create a destination from an existing activity or fragment.

`Start Destination:` The start destination is the firs screen when opening your app, and its the lasst screen users see when exiting the app. The navigation editor uses a house icon to indicate the start destinations. 

`PlaceHolder Destinations:`represent unimplemented destinations. Aplaceholder serves as a visual representation of a destination. Within the navigation Editor, you can use placeholdersr just as you would any other destination. 

`Dialog Destination:` If you have an existing Dialog Fragment, you can use the dialog element to add the dialog to the navigation graph. 

## Actions
`Actions:` Actions connect one Destination to another, they are represented in the navigation graph as arrows and can have multiple paths.

`Navcontroller:` is an object that manages app navigation within a NavHost. Each NavHost has its own corresponding NavController. you can retireve a Navcontroller with the following. 
- Fragment.findNavController()
- View.findNavController
- activity.findNavController()



`Top-LevelNavigation:` Top-level navigation graphs should start with the initial destination the user sees when launching he app and should include the destinations that thye see as they move about your app. 






