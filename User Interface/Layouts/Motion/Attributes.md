# Interpolated attributes
Witin a MotionScene file, `ConstraintSet` elements can contain additional attributes that are interpolated during transition. In addition to positioon and bounds, the following attributes are interpolated by `MotionLayout`.

- alpha
- visibility
- elevation
- rotation, rotationX, rotationY
- translationX, translaytionY, translatioinZ
-scaleX, scaleY


# Custom Attributes
Within a `<Constraint>`, you can use the `<CustomAttribute>` eleemtn to specify a transition for attributes that aren't simply related to position or View attributes. 
```
<Constraint
    android:id="@+id/button" ...>
    <CustomAttribute
        motion:attributeName="backgroundColor"
        motion:customColorValue="#D81B60"/>
</Constraint>
```

A `<CustomAttribute>` continnas two attributres of its own. 
  - `motion:attributeName` is required and must match an object with getter and setter methods. Th getter and setter must match a specific patter. For example backgroundColor is supported, since our view had underlying getBackgrounColor9) and setBackgroundColor() methods. 
  
`motion:customColorValue` for colors
`motion:customIntegerValue` for integers
`motion:customFloatValue` for floats
`motion:customStringValue` for strings
`motion:customDimension`for dimensions
`motion:customBoolean` for booleans

Note: that when specifying a custom attribute, you must define endpoint values in  both the start and end `<ConstraintSet>` elements. 
