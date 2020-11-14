# Tool tip
A tooltip is small descriptive message that appears near a view when users long press the view or hover their mouse over it. This is useful when your app uses an icon to represent an action or piece of information to save space in the layout. This page shows you how to add these tooltips.

Some scenarios, such as those in productivity apps, require a descriptive method of communciating ideas and actions. You can use tooltips to display a descriptive message 

Some standard widgets display tooltips based on the content of the title or content description properties. 

## Setting the tooltip text
You can specify the tooltip text in a View by calling the setTooltipText() method. You can set the `tooltipText` property using the corresponding XML attribute or API

To speify the tooltip textin your XML files, set the `android:tooltipText` attribute 
```
<android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:tooltipText="Send an email" />
```

To speicfy the tooltip textin your code, us the seetTooltipText(CharSequenece) 
```
val fab: FloatingActionButton = findViewById(R.id.fab)
fab.tooltipText = "Send an email"
```

The API also includes a `getTooltipText()` method that you can use to retrieve the value ofthe `tooltipText` propety.

Android displays the value of the tooltipText property when users hover their mouse over the view or long press the view. 
