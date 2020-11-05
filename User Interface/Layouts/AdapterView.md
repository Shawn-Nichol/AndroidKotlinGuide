# AdapterView
AdapterView is a `ViewGroup` that displays items loaded into an adapter. The most common type of adapter comes from an array-based data source. 

## Fill the layout with data
TO add data into the layout, add code similar to the following. 
```
val PROJECTION = arrayOf(Contacts.People._ID, People.NAME)
...


// Get a Spinner and bind it oto an ArrayAdapter that references a String array. 
val spinner1: Spinner = findViewById(R.id.spinner1)

val adpater1 = ArrayAdapter.createFromResource(this, R.array.colors, android.R.layout.simple_spinner_item)
adapter1.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
spinner1.adpater = adpater1

// Load a spinner and bind it to a data query. 
val spinner2: Spinner  = findViewById(R.id.spinner2)
val cursor: Cursor = managedQuery(People.CONTENT_RUI, PROJECTION, null, null, null)
val adapter2 = SimpleCursorAdapter(this, 
  // Use a template that displays a text view
  android.R.layout.simple_spinner_item,
  // Give the cursor to the list adapter
  cursor, 
  // Map the NAME column in the people database to...
  arrayOf(People.NAME),
  // The "text1" view defined in the XML template
  intArrayOf(android.R.id.text1))
adapter2.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)
spinner2.adapter = adapter2
```

Note that it is necessary to have the PEOPLE_ID column in projection used with CursorAdapter or else you will get an exception.

If, during the life of the application, you can change the under lying data that is read by your Adapter you should call notify `DataSetChagned()`. This will notify the attached `View` that the data has been changed and it should referesh itself. 


## Handler Selections
You handle the user's selection by setting the class's `AdapterView.OnItemClickListener` member to a listener an catching the selection changes. 

```
val historyView: ListView = findViewById(R.id.history)
historyView.onItemClickListener = AdapterView.OnItemClickListener { parent, view, position, id ->
    Toast.makeText(context, "You've got an event", Toast.LENGTH_SHORT).show()
}
```
