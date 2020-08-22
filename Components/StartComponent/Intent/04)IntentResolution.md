## Intent resolution
When the system receives an implicit intent to start an activity, it searches for the best activity for the intent by compaing it to intent filters based on three aspects
- Action
-  Data
- Category

Action test
To specify accepted intent actions, an intent filter can declare zero or more ,action> elements. To pass this filter, the action specified in the Intent must match one of the actions listed in the filter
If the filter does not list any actions, there is nothing for an intent to match, so all intents fail the test. However, if an intent does not specify an action, it passes the test as long as the filter contains a least one action. 

Category Test
To specify accepted intent categories, an intent filter can declare zero or more <category> elements. For an intent to pass the category test, every category in the Intent must match a category in the filter. The reverse i not neccessary- the intent filter may declare more categories than  are specified in the Intent and the Intent still passes. Therfore an intent with no categories alwasy passes this test, regardless of what categories are declared in the filter. 
 
 Data Test
 To specify  accepted intent data, an intent filter can declare zero or more <data> elements. Each <data> element can specify a URI structure and a data type. Each part of the URI is a separate attribute: Scheme, host, port, and path
