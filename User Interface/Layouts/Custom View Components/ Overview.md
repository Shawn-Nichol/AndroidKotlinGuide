# Custom View Components
Android offers a sphisticated and powerful componentized moduel for building your UI, based on the fundamental layout classes: `View` and `ViewGroup`. To start with the platform includes a variety of rebuilt View and ViewGroup subclasses called widgets and layouts, respectively that you can use to construct your UI.  

If non of the prebuild widgets or layouts meets your needs, you can create your own View subclass. If you only need to make small adjustments to an existing widget or layout, you can simply subclass the widget or layout and override its methods. 

Creating your own View subclasses gives you precise control over the appearance and function fo a screen element. To give an idea of the control you get with custom views, here are some examples of what you could do with them. 

- You could create a completely custom-rendered View type, for example a "Volume control knob rendered using 2D graphics and which resemeles an analog electronic control 
- YOu could combine a group ovf View components into a new single component, perhaps to make something like a ComboBox(a combination of popup list and free entry text field), dual -pane selector control (a left and right pane with a list in each where you cna reassign which item is in which list and so on

- You could override the way that an EditText component is  rendered on the screen
- You could capture other events like key presses an dhandle them in some custom way 

The sectoions below explan how to create custom Views and use them in your application. For detailled reference information see the `View class. 

## The basic Approach
Here is a high level overview of what you need to know to get started in creating your own View components:
1. Extend an existing `View class or subclass with your own class
2. Override some of the methods from the superclass. The superclass methods to override start with on for example, onDraw(), onMeasure(), and onKeyDown(). This is similar to the on... events in the Activity or ListActivity that you override for lifecycle and other functionality hooks. 
3. Use your new extension class. Once completed, your new extension class cn  be used in place of the view upon which it was based. 

Tip: Extension classes cvan bedefined as inner classes inside the activities that use them. This is useful because it control saccess to them but isn't necessary. 

## Fully Customized components. 
Fulllly customized components can be used to create grphical components that appear however you wish. Perhaps a words so you can sing along witha karaoke machine. Either way, you want something that the built-in components just won't do, no matter how you combine them. 

Fortunately, you can easily create components that look and behave in any way you like, limited perhapas only by your imagination, the size of the screen, and theavailable processing power (remember that ultimately you application might have to run on something with significantly less power than your desktop workstation

To create a fully customized ocmponent
1. The most generic view you can extend is, unsurprisingly, view, so you willl ususally start be extending this to create your new super component. 
2. You can supply a constructor which can take attributes and paramters from the XML, and you can also consume your own such attributes an parameters(perhaps the color and range of the VU meter, or the width and damping of the needle. )
3. You will probably want to create your own event listeners, property accessors and modifiers, and possibly more sophisticated behavior in your component class well. 
4. You will almost certainly want to overrid `onMeasure()` and are also likley to need to override onDraw() if you want the component to show something. While both have default behavior, the default onDraw() will do nothing and the default onMeasure() will alwasy set a size of 100X100 which is probalby not what you want. 
5. Other `on...` methods may also be overridden as required. 

## Extend onDraw() and onMeausre()
The onDraw() method delivers you a Canvas upon which you ca nimplement anything you want: 2D graphics, other standard or custom compoennts, styled text, or anything else you can think of. 

Note: This does not apply to 3D grpahics. If you want to use 3D graphics, you must extend SurfaceView instead of View, and draw form a separate thread.

`onMeasure()` is a littl emore invlolved. `onMeasure()` is a crivtical piece of the rendering contaract between your component and its container. `onMeasure()` should be overridden to efficiently and accurately report the measurements of its contained parts. This is made slightly more complex by the requirements of limits from the paretn(whicha re passed in to the `onMeasure()` and by the requirement to call the `setMeasuredDimension()` with the measured widht and height once they have been calculated. If you fail to call this method from an overrideen `onMeasure(0, the result will be an exception at measurement time. 

At a high level, implemenint `onMeasure() looks something like this. 

1. The overriden `onMeasure()` mehtod is called with width and height measure specifications (widthMeasureSpec and heightMeasureSpec parameters, both are integer codes representing dimensions) which should be treated as requirements for the restrictions on the width and height measurements you should produce. A full reference to the kind of restrictions these specifications can require can be found in the reference documentation under `View.onMeasure(int, int)` this reference ocumentation does a pretty good job of explaining the whole measuremnt operation as well
2. Your component's `onMeasure()` should calculate a measuremnt widht and height wich will be required to render the component. It should try and stay within the specifications passed in, although it can choose to exceed them (in this case, the parent can choose what to do, inclduing clipping, scrolling, throwing an exception, or asking the onMEasure() to try again, perhaps with differen measurement specifications). 
3. Once the width and height are calculated, the setMeasuredDeimension(int width, int height) method must be calle diwth the calculated measurements. Failure to do this will result in an exception being thrown. 

## Compound Controls
If you don't want to create a completely customized omponent, but instead are looking to put together a reusable component that consists of a group of existing controls, then creating a Compound Component (or Compound Control) might fit the bill. In a nutshell, this brings together a number of more atomic controls into a logical group of items that can be treated as a single thing. For example, a Combo Box can be throught of as a combination of a single line EditText field and an adjacent button with an attached PopupList. If you press the button and select somethign from the list, it populates the EditText fild, but the user can also type something directly into the EditText  if they prefer. 

In Android there are actually tow other Views readily available to do this `Spinner and `AutoCompleteTextView, but regardless, the concept of the Combo box makes an easy-to-understand 

### Create a compound
1. The usuall starting point is a layout of some kind, so create a class that extends a lyout. Perhaps in the case of a bombo box we might use a LinearLayout with horizontal orientation. Remember that other layouts can be nesteed inside, so the compound component can be arbitrarily complex and structured. Note that just like with an Activity, you can use either the declarative approach to creating the contained compoents, or you can nest them progamtically from your ocde. 

2. In the constructor for th new class, take whateer parameters the superclass expects, and pass them through to the superclass constructor first. Then you can set up the other views to use within your new component; this is where you would create the EditText field and the PopupList. Note that you also might introduce your own attributes and parameters in to the XML that can be pulled out and used by your constructor. 

3. You can also create listeners for events thrat your contained views might generate, for example a listener method for the list item click listener to update ht contents of the EditText if a list selection is made. 

4. You might also create your own properties with accessors and modifiers, for example, allow the EditText value to be set initially in the component and query for its contents when needed. 
5. In the case of extending a Layout, you don't need to override the `onDraw()` and `onMeasure() methods since the layout will have default behavior that will likely work just fine. However you can still override them if you need to. 

6. You might override other `on...` methods, like `onKeyDown()`, to perhpas choose certain default values fromt he popup list of a combo box when a certain key is pressed. 

To summarize the use of a Layout as the basis for a Custom Control has a number of advantages. 

- You can specify the layout using hte delcarative XML files just like with an activity screen, or you can create views programmatically and nest them into the layout from your code. 

- The `onDraw()` and `onMeasure()` mehtods (plus most of the other `on...` methods) will likely have suitable behavior so you don't have to override them. 

- In the end, you can very quickly construct arbitrarily complex compound views and re-ues them as if they were a single component. 


## Modifying an Exisitng View Type. 
