# Navigate between fragmetns uisng animations
The fragment API provides two ways to use motion effects and transformations to viually connecty fragments during navigation. One of these is the Animation Fraemwork, which uses both Animation and Animator. The other is the Transition Framework, which includes shared element transistions. 

Note in this topic, we use the term animation to descibe effects in the Animation Framework, and we use th term tansition to describe effects in the Transition framework. These two frameworks are mutually exclusive and should not be used at the same time. 

You can specify custom effects for entering and exiting fragments and for tansitions of shared elements between fragments.
- An enter effecter determines how a frament enters the screen. For example you can create an effect to slide the fragment in from the edge of the screen when you navigate to it. 
- An exit efffect dtermines how a fragment exits the screen. For example, you can create an effect to fade the fragment out when navigating away from it. 
- A shared element transition determines how a view that is shared between  two fragments moves between them. For example, an image displayed in an ImageView in fragment A transitions to fragment B once B becomes visible. 

## Set animations
First, you need to create animations for your enter and exit effects, which are run when navigating to a new fragment. You can define animations as tween animations resources. These resources allow you to define how fragments should rotate, strtch, fade, and move during the animation. For example, you might want the current fragment to fade out and the new fragment to slide in from the right edge of the screen 

Tween animition: are defined in XML and perform transitions such as rotating, fading, moving and strectching on a graphic. 

popEnter and popExit Animation: animation for enter and exit effects that are run when popping the back stack. 

## Set transistions
You can use transistions to define enter and exit effects. These transitions can be defined in XML resource files. For exmple, you might want the current fragment ot fade out and the new fragment to slide in from the right edge of the scree. Once the transistion is defined you apply theym by calling setEnterTransition() on the entering fragment and setExitTransition() on the exiting fragment, passing in you rinfalted transistion resouvres by their resource ID,
