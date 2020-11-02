# KeyFrame animations.
Within a ConstraintLayout, you can animate changes to the size and position of elements by using `ConstraintSet` and `TransitionManager`. A `ConstraintSet` is a light weight object that represents the constraints, margings and padding of all child elements within a `ConstraintLayout`. When you apply a `ConstraintSet` to a displayed `ConstraintLayout`, the layout updates the constraints of all of its children.


## Build a key Frame. 
1. Create your layout file

2. Create a second layout file where you want your images to move to. 

3. Add the animation in the activity or fragment. 
Fragment
```
override fun onCreateView(...): View? {
    binding = DataBindingUtil.inflate(inflater, R.layout.fragment_menu_three, container, false)

    // Refernced the ConstraintLayout
    val constraint = binding.constraint_layout_one

    // Represents the constraints, margins and padding of all child elements within a ConstraintLayout.
    val constraintSet = ConstraintSet()
    
    // Load: a constraint set from a constraintSet
    constraintSet.load(activity, R.layout.fragment_menu_three_2)


    // This class manages the set of transitions that fire when there is a change of Scene. Tl use the manager
    // add scenes along with transition objects with calls to setTranition. 
    TransitionManager.beginDelayedTransition(constraint)
    constraintSet.applyTo(constraint)


    return binding.root
}
```

Note: ConstraintSet requires all child views have IDs.
