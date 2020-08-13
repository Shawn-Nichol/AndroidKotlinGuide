# Custom Attributes

YOu can use a CUstomAttrbute so specify any other attribute. A customAttribute can be used to set any value that has a setter. For example you can set the backgroundColor on a View using a customAttribute. MotinoLayout willl use reflection to find the settter, then callit repeatedly to aniamte the view. 

Custom attrbute is added inside a KeyAttribute. The custom Attribute will be applied at the framePosition specifed by teh KeyAttribute. Insidee the CustomAttribute you must specify an attributeName and one value to set. 

- app: attributeName ist eh name of the setter that will be called by this custom attribute. 
- app: custom value is a custom value of the type noted in the name. Custom value can have any of the following. 
  - color
  - Integer
  - Float
  - String 
  - Dimension
  - Boolean

As long as a MotionLayout can find a setter taht takes the correct type it can animate changes between the values. 
