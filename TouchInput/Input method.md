# Specify the input method
Every text field expects a certain type of text input, such as an email address, phone number or just plain text. So it's important that you specify the input type for each text filed in your app so the system displays the appropriate soft input method such as an on-screen keyboard.

Byond th type of buttons available with an input method, you should specify behaviors such as whether the input method provides spelling suggestions, capitalizes  new sentences, and replaces the carriage return button with an action button such as a `Done` or `Next`

## Specify the keyboard type
You should alwasy delcare the input method for your text fields by adding the `android:inputType` attribute
```
<EditText
    android:id="@+id/phone"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:hint="@string/phone_hint"
    android:inputType="phone" />
```

## Enable spelling suggestions and other behaviors
The `android:inputType` attrbiute allows you to specify various behaviors for the input method. Most importantly, if your text field is inntended for basic text input, you should enable auto spelling correction with the "textAutoCorrect" value.

You can combine different behaviors and input methods styles with the `android:inputType` attribute. 
```
<EditText
    android:id="@+id/message"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:inputType=
        "textCapSentences|textAutoCorrect"
    ... />
```

## Specify the input method action
Most soft input methods provide a user action button in the bottom corner that's appropriate fo the current text field. By default, the system uses this button for either a Next or Done action unless your text field allows multi-line text (such as with `android:inputType="textMultiLine"`), in which case the action button is a carriage return. However, you can specify additional actions that might be more appropriate for your text field, such as Send or GO

To speicfy the keyboard action button, use the `android:imeOptions` attribute withan action value such as `actionSend` or `actionsearch`.

```
<EditText
    android:id="@+id/search"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:hint="@string/search_hint"
    android:inputType="text"
    android:imeOptions="actionSend" />
```

you can then listen for presses on teh action buttotn by defining a `TextView.OnEditorActionListener for the EditText elemetn. in your listener, respond to the appropriate IME action ID defined in the `EditorInfo` class, suchas as `IME_ACTION_SEND`.
```
findViewById<EditText>(R.id.search).setOnEditorActionListener { v, actionId, event ->
    return@setOnEditorActionListener when (actionId) {
        EditorInfo.IME_ACTION_SEND -> {
            sendMessage()
            true
        }
        else -> false
    }
}
```

## Provide auto-complete suggestions
If you want to provide suggestions to users as they type, you can use a subclass of `EditText` called `AutoCompleteTextView`. To implement auot-complete, you must specify an `Adapter` that provides the text suggestions. There are several kinds of adapters available, depending on where the data is coming from, such as from a database or an array. 

The following procedure describes how to set up an `AutoCompleteTextView` that provides suggestions from an array, using `ArrayAdapter`
1. Athe `AutoCompleteTextView` to your layout.
```
<?xml version="1.0" encoding="utf-8"?>
<AutoCompleteTextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/autocomplete_country"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
```
2. efine the arry that contains all text suggestions. 
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="countries_array">
        <item>Afghanistan</item>
        <item>Albania</item>
        <item>Algeria</item>
        <item>American Samoa</item>
        <item>Andorra</item>
        <item>Angola</item>
        <item>Anguilla</item>
        <item>Antarctica</item>
        ...
    </string-array>
</resources>
```

3. In your `Activity` or `Fragment`, use the following code to specify the adapter that supplies the suggestions. 
```
// Get a reference to the AutoCompleteTextView in the layout
val textView = findViewById(R.id.autocomplete_country) as AutoCompleteTextView
// Get the string array
val countries: Array<out String> = resources.getStringArray(R.array.countries_array)
// Create the adapter and set it to the AutoCompleteTextView
ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, countries).also { adapter ->
    textView.setAdapter(adapter)
}
```
Here, a new ARrayAdapter is initialized ot bind each item in the `countries_arrary` string array to a `TextView` that exists in teh `simple_list_item_1`

4. Assign the adapter to the `AutoCompleteTextView` by calling `setAdapter()`
