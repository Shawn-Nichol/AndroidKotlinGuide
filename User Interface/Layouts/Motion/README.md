# What is a MoionLayout
`MotionLayout is a lyout type that helps manage motion and widget animation. `MotionLayout` is a subclass of `ConstraintLayout`. `MotionLayout` bridges the gap between layout transitions and complex motion handling, offering a mix of freatures between the property animation frameowrk, 'TransitionManager` and `CoordinatorLayout`. `MotionLayout` is fully declarative, meaning that you can describe any transitions in XMl, no matter the complexity. 

## Design considerations
`MotionLayout` is intended to move, resize and animate UI elements with which users interact, such as buttons and title bars. Motion in your app should not be simply a gratuitous special effect in your application. It should be used to help users understand what your application is doing. 

`<Transistion>` Specifies the beginning and end state of the motion sequence, and desired intermediate states, and the user interaction taht trigger the motion.
  - `motion:constraintSetStart` and `motion:constraintSetEnd` are references to the start and endpoints of the motion. These endpoints are defined in the `<ConstraintSet>` elments layer in the MotinoScence
  - `motion:duration` specifies the number of milliseconds that it takes for the motion to complete. 
  
`<OnSwipe>' lets you control the motion via touch. 
  - 'motion:touchAnchorId` referes to the view that you can swipe and drag.
  - 'motion:touchAncorSide' means that user are draggin the view from the right side. 
  - `motion:touchAnchorSide` means that we are dragging the view from the right side. 
  - 'motion:dragDirection` referes to the progress direction of the drag.
  
`<onClick>` specifies the action to perfrom when the user taps on a specific view. There can be multiple <onCLick> nodes for a single <Transition>, with each <onClick> specifying a different target view and a different action to perfrom when the veiw is tapped. 

  
`<ConstraintSet>` Specifies the position and attributes of all of the views at one point in a motion sequence. Typically, a `<Transition>` element points to two `<ConstraintSet> elements, one defining the beginning of the motion sequence and one defining the end. 

`MotionScene` Root element of a XML resource file scene file, to keep layout information separate from motion descriptions, each `MotionLayout` references a separate MotionScene. Note that defintions in the MotionScene take precendence over any similar definitions in the `MotionLayout`. 

`<Constraint> Specifies the location of the element of the motion sequence. 

`<KeyFrameSet>` Specifies location and attributes for views over the course of the motion sequence. By default, motion proceeds from the initial state to the end state; by using<KeyFrameSet>, you can build more complex motions. The <KeyFrameSet>Contains <KeyPosition> or <Key Attribute> nodes. Each MotionLayout smoothly animate the view from the starting point to each of those intermediate points, and then to the final destination. 

`<KeyPosition>` Specifies a view's position at a specific moment during the motion sequence. This attribute used to adjust eh default path of the motion.

`<KeyAttribute>` Specifies view attributes at a specific moment during the motion sequence. `<KeyAttribute>` can be used to set any the view's standart attributes. 

Description of all the elements and their attributes can be found here. 
https://developer.android.com/reference/androidx/constraintlayout/motion/widget/MotionLayout

