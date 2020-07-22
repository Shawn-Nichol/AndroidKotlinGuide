To Create a fully cutomized component:
- Most generic view you can extend is 'View', start by extending this to create a new super component. 
- You can supply a constructor which can take attribute and parameters fromt eh XML, and you can also consume your own such attributes an parameters(perhpas the color
and range of the VU meter, or the width and damping of the needle
- You can can create your own event listeners, property accessors and modifiers, and possibly more sophisticated behavior in your component class as well. 
- You will want to override onMeasure and onDraw() if you want your component to show something.
- Other on methods may also be overridden

Extend onDraw() and onMeasure()
