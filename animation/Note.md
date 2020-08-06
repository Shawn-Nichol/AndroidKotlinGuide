## Property
A property, to the animation system, is a field that is exposed via setters and getters, either implicityly or explicitly. There are also a special case of properties exposed via the class android.util.Property which is used by the View class, which allows a type-safe approach for animations. The animator system in Android was specifically written to animate properties, meaning that it can animate anything that has a setter

## View Properties are a sest of functinality added to the base View class that allow all views to be transformed in specific ways that are useful in UI animations. There are properties for position rotation, scale, and tranparency

There are actually two different ways to access the properties by regular setter getter pairs, like setTranslateX()/ getTranslateX() and by static android.util.Propety objects, like View.TRANSLATE_X. 

## pROPETY ANIMATION 
Is simply the changing of property values over time. ObjectAnimator was created to provide a simple set-and-forget mechanism for animating properties. For example you can define an animator that changes the alpha property of a View, which willl alter the transparency of that view on the screen. 


## handle request for animation during an animation
If prompted for another animation in middle of animation, the animation will jump to the being. To avoid this you can use the Animators listeners, which call back into user code to notify the appliation fo changes in the state of the animation. There are callbacks for an animation starting, ending, pausing, resuming, and repeating. What 


## Repetition is a way of telling animations to do the same task again and again. You can specify how many timse to repeat(or just tell it to run infinitley ). You can also specify the repetition behavior, either REVERSE(for reversing the direction every time it repeats) or RESTART (for animating from the orignial start value to the orignal end value, thus repeating in the same direction everytime

If the repeat animation is started during an animation, the animation will continue from its current start position and return to the position where the position where the animation was request. This is due to the fact that a start position wasn't declared in the animation. 

