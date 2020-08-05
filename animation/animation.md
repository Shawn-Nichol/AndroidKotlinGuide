### property
Property to the animation system, is a field that is exposed via setters and getters, either implicityly or explicityly(viwa the setter/getter pattern in the Java programming language). There are also a special cases of properties exposed via the class android.util.propety which is used by the view class, which allows a type-safe approach for animations. The animator system in Android was specifically written to animnate properties, meaning that it can animate anything that has a setter. 

### ViewProperties
ViewProperties are a set of functionality added to the base View class that allow all views to be transformed in specific ways that are useful in UI animations. There are properties for position rotations, scale and transparency. 

There are actually two different ways to access the propeties by regular setter/getter pairs, like setTranslateX()/getTranslateX(), and tby static android.util.Property objects, like View.Translate_X(an object that has both a get() and a set() method). Android.util.Property have less overhead internally along with better type-safety, but you can also use the setters and getters of any object. 

### Property animation
Property animation is, the change of property values overt time. ObjectAnimator was created to provide a simple set and forget mechanism for animating properties. For example, you can define an animator that changes the alpha property of a view, which will alter the transparency of that view on the screen.
