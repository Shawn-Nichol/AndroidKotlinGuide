# Design for different form factors

The desing of your app's UI isn't tied to aparticular device form factor. Android applications need to apdapt to a number of different types of devices, from 4-inch handsets of 50-inch TVs to chrome OS devices with resizable windows. 

Your app's user interface is drawn inside of a window, the size of which can change at will. You use reource qualifiers to privde idfferent layouts for varying window sizes. These diferences can be due to constraints in the size of the deivce's screen, or they can be driven by the user using multi-window mode to change the window size. 

## Disgingin responsive content
you should proide a rich experince fo all of your users, so you should have each screen in your app take full advantage of the window real estate avaiable to you. 

Fore example an app running in a window taking up the full width of a phone screen could perhaphs hide deteails for a piece of content when entering multi-window mode, and it could expand its user interface to provide more content when running in a window taking up the full width of a chorome OS device screen. 

In addition to addressing these user expectations, it's often neceessary to provide more content on larger devices to avoid leaving to much whitespace or un wittingly introduing awkward interactions. In the following figure, you can see some of the problems that can ariase when adapting a user interface design for a larger window. 


## Providing taileored USer experinces

Its important to provide unique experineces that go beyond expanding your contents views to fill avaiable space. you can tayilor user interfaces to provide th ideal user experince for given window sizes, even using enterly different layouts and widgets. 

BottomNavigationView is used as top-level navigation when there is adequate vertiacl space to do so. When the size of the window is reduced, top level navigation is instead implemented using a Drawerlayout

Here are some other examples.

- A toolbar can show or hide action menu items given the amount of avaiable space. 
- A recyclerView.LayoutManager could change its span count to take full advantage of size of a window. 
- You can increase the amount of detail you show for custom views as you have more space to do so. 

These are all great ways to make sure that your users have great experiences wherever they're running your app

