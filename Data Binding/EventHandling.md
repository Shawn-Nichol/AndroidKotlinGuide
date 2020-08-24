# Event Hanlding

Data binding allows you to write expression handling events that are dipatched from the views(for example the onCLick()). Event attribute names are determined by the name of the listener method witha few exceptions. For exampel, View.OnClickListener has a method onClick()), so the attribute for this event is android:onClick to avoid a conflict. you can use thfollowing attributes to avoid these type of conflicts. 

You can use the following methods to handle an event

Method reference: 
In your expressions, you can reference methods that conform to the signature of the listener method. When a expression evaluaates to a method reference, data binding wraps the method reference and owener object in a listener, and sets that listener on the target view. If the expression evaluates to null, Data binding doesn't create a listener and sets a null listener instead.

Listener binding: 
These are lambda expression that are evaluated when the event happens. Data binding alwasy creates a listener, which it sets on the view. When the event is dipatched, the listener evalutates the lambda expression

Method references

Events can be bound to handler emthods directly, similar to the way android:onClick can be assigend to a method in an activity. one major advantage compared to the view onClick attribute is that the expression is processed at compile time, so if the method doesn't exist or its signature is incorrect, you receive a compile time error. 

The major difference between method references and listener bindings is that the actual listener implementation i screated when the data is bound, not when the event is triggered. if you prefere to evaluate the expression when the event happens, you should use listner binding

To asiign an even to its handler, use a normal binding expression,with the value being the method name to call. 

```
class My Handlers{
  fun onClickFriend(view: View) {
    ...
  }
}
```

The binding expression can assign the click listener for a view to the onClickFriend()
```
<layout
  ...>
  <data>
    <variable name="handlers" type="com.example.handler"/>
  </data>
  
  <LinearLayout
  ...>
    <TextView
      ...
      android:onClick="@{handlers::onClickFriend}"/>
  </Linearlayout>
</layout>
```

## ListnerBinding
Listener binding are binding expressions that run when an event happens. They are similar to method references, but they let you run arbitrary data bidning expressions.

In method references, the parameters of the method must match the parameteres of teh event listener. In listener binding, only your return value must match the expected return value of the listener.
```
class Handlers
  fun onSaveClick(task: Task)
```

You can bind the click even to the onSaveClick()
```
<layout
  ...>
  <data>
    <variable name="handlers" type="com.example.handler"/>
  </data>
  
  <LinearLayout
  ...>
    <TextView
      ...
      android:onClick="@{() -> handlers.onSaveCLick(task)}"/>
  </Linearlayout>
</layout>
```

When a callback is used in an expression, data binding automatically creates the necessary listener and registers if for the event, data binding evaluates the given expression. As in regular binding expressions, you still get null and thread safety of data binding while these listener expressions are being evalutated. 

In the example above, we 
