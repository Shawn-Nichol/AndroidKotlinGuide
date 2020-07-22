To Create a fully cutomized component:
- Most generic view you can extend is 'View', start by extending this to create a new super component. 
- You can supply a constructor which can take attribute and parameters fromt eh XML, and you can also consume your own such attributes an parameters(perhpas the color
and range of the VU meter, or the width and damping of the needle
- You can can create your own event listeners, property accessors and modifiers, and possibly more sophisticated behavior in your component class as well. 
- You will want to override onMeasure and onDraw() if you want your component to show something.
- Other on methods may also be overridden

## Extend onDraw() and onMeasure()
- onDraw(): method delivers a Canvas upon which you can implement anything you want: 2D graphics, other standard or custom components, styled text or anthing else you can think of. 
- onMeasure() is a critical piece of the rendering contract between your component and its container. onMeasure() should be overridden to efficiently and accurately report he measurements of its contained parts. This is made slightly more complex by the requirements of limits from the parent and by the requirement to call teh setMeasuredDimension() method witht the measured width and height once they have been calculated. If you fail to call this method from an overridden onMeasure() method, the result will be an exception at measurment time. 

Implement onMeasure()
- method with width and height measure specifications parameters, both are integer codes representing dimensions)  which should be treated as requirements for the restrictions on the width and height measurements you should produce. 
- onMeasure() should calculate a measurement widht and height wich will be required to render the component. It should try to stay within the specifications passed in, although it can choose to exceed them. 
- Once the width and height are calculated, teh setMeasuredDimension(int width, int height) method must be called with the caculated measruements. Failure todo this will result in an exception being thrown. 

## Compound Controls 
If you don't want to create a completely customized componetn, but instead are looking to put toether a reusable componet that consist of a group of existing controls, then  create createing a Compound COmponent might work. This bring together a number o f more atomic controls into a logical group of items that can be treated as a single thing. For example a Combo Box can be thourght of as a combination of a single line EditText field and an adjcent button with an attached PopupList. If you press th button and select someting fomr the list, it populates the EditText field, but the user can also type something directly into the EditText if they prefer. 

Create a compound component:
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
