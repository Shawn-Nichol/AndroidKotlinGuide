## Float Action Button (FAB)
A floating action button is a circular button that triggers the primary action in your app's UI. FAB come in three types regular, mini, extended. Only use an FAB as a suitable way to display the screens primary action, there should only be one FAB on the screen.



Placement
FABs can attach to top app and the edge of some components, the top app.
Individual components, such as cards should not have  their own FAB. 

Behavior
A FAB can trigger an action either on the current screen, or it can perform an action that creates a new screen. 
FAB should promote important, constructive actions
- Create
- Favorite
- Share
- Start a process

Motion
When a FAB animates on screen, it expands outwards from a central point. The icon within it may be animated as well. While FABs should be relevant to screen content, they aren't attached to the surface on which content appears. FABs move separately from other UI elements becuase of their relative importance. 

Screen Transistions
Fabs can morph to launch rlateed actions. When screen changes its layout, the FAB should disappear and reappear during the transistion. 

Speed Dial
When pressed, a FAB can display three to Six related actions in the form of a speed dial. This transition can occur in one of the following ways:

- Upon press, the FAB can emit related actions
- Upon press, the FAB can transform into a menu containing related actions

If more thatn sixx actions are needed, somethign other than a FAB should be used to present them.

Extended FAB
THe extend FAB is wider, and it includes a text label.
