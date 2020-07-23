# How to create a Custom View
1) to Create a custom view of any size and shape, add a new class that extneds View. 
2) Override 'View' methods such as 'onDraw()' and 'onMeasure()' to define the view's shape and basic appearnce. 
3) Use invalidae() to force a draw or redraw of the view. 
4) To optimize performance, assign any required values of drawing and painting before using them in onDraw(), such as in the constructor or init() helper method
5) Add listeners to custom view interactions
6) Add the custom view to an XML lalyout file with attributes to define its  apperaance, as you would witih other UI elements. 
7) Create the Attrs.xml file in the values folder to define custome attributes. You can then use the custom attributes for the custom view in teh XML layout file. 


## Extend onDraw() and onMeasure()
- onDraw(): delivers a Canvas to which you can implement anything you want in 2D,

- onMeasure() is a critical piece of the rendering contract between your omponent and its container. overriding onMeasure() will efficiently and accurately report the measurements of its contained parts. This is made slighly more complex by the requirements of limits from the parent and by the requirement to call the setMeasuredDimension() method with the measured width and height once they have been calculated. If you fail to call this method from an overriden onMEasure() method, the result will be an exception at measuremnt time. 

### Implement onMeasure()
- width and height measuremets are specifications parameters, both are integer codes representing dimensions)  which should be treated as requirements for the restrictions on the width and height measurements you should produce. 

- onMeasure() should calculate a measurement width and height which will be required to render the component. It should try to stay within the specifications passed in, although it can choose to exceed them. 

- Once the width and height are calculated, the setMeasuredDimension(int width, int height) method must be called with the caculated measruements. Failure todo this will result in an exception being thrown. 

## Compound Controls 
If you don't want to create a completely customized component, but instead are looking to put toether a re-usable componet that consist of a group of existing controls, then a Compound Component might work. This brings together a number of more atomic controls into a logical group of items that can be treated as a single thing. For example a Combo Box can be thourght of as a combination of a single line EditText field and an adjcent button with an attached PopupList. If you press the button and select someting from the list, it populates the EditText field, but the user can also type something directly into the EditText if they prefer. 

# Create a compound component:
- create a class that extends a layout, other layouts can be nested inside. Just like with an Activity you can use either the declarative (XML-based) approach to creating the contained componenets, or you cn nest them programmatically. 
- In the constructor for the new class, take whatever parameters the superclass expects, and pass them through to thesuperclass constructor first. Then you can set up the other views to use within your new component: this is where you would create the EditText field and the PopupList. Note that you also might introduce your own attributes an parameters into the XML that can be pulled out and used by your constructor. 
- You can also create listeners for events that your contained views might generate. 
- you can create your own properties with accessors and modifiers. 
- IN the case of extending a Layout, you don't need to override onDraw() and onMeasure() methods since the layout willl have default behavior that will likely work jsut fine. However, you can still overrid ethem if you need to
- you can override other on methods

Summarize layout
- you can specify the layout using the delarative XML files just like with an activity screen or you can create views programmatically and nest them into the layout from your code. 
- onDraw and onMeasure methods will likely have sutiable behavior so you don't have to override them
- 
