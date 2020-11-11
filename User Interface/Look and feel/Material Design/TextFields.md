# Text fields
Text fields allow users to enter text into a UI. They typically appear in forms and dialoggs. 

- Text fields should stand out and indicate that users can input information.
- Text field states should be clearly differentiated from one another. 
- Text fields should make it easy to understand the requested information and to address any errors. 

There are two types 
- Filled
- OutLined


Setup TextInputlayout
```
<com.google.android.material.textfield.TextInputLayout
    android:id="@+id/last_name"
    style="@style/TestInput"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="@string/last_name"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/new_first_name">

    <com.google.android.material.textfield.TextInputEditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="textPersonName"
        />
</com.google.android.material.textfield.TextInputLayout>

```


Set the style for TextInputLayout so it doesn't have to be repeated for every TextInputLayout.
```
    <style name="TestInput" parent="Widget.MaterialComponents.TextInputLayout.FilledBox">
        <item name="hintTextAppearance">@style/App.hintTextTextAppearance</item>
        <item name="helperTextTextAppearance">@style/App.helperTextAppearance</item>
        <item name="hintTextColor">@color/black</item>
        <item name="android:paddingBottom">16dp</item>
        <item name="boxStrokeColor">@color/colorAccent</item>
        <item name="maxLines">1</item>
        <item name="helperText">@string/first_name</item>
        <item name="android:colorControlHighlight">@color/orange</item>
        <item name="counterMaxLength">20</item>
        <item name="endIconMode">clear_text</item>
        <item name="android:layout_marginTop">16dp</item>
        <item name="android:layout_marginStart">16dp</item>
        <item name="android:layout_marginEnd">16dp</item>
        <item name="android:textSize">16sp</item>
        <item name="errorEnabled">true</item>
    </style>


    <style name="App.hintTextTextAppearance" parent="TextAppearance.MaterialComponents.Caption">
        <item name="android:textSize">15sp</item>
    </style>

    <style name="App.helperTextAppearance" parent="TextAppearance.MaterialComponents.Caption">
        <item name="android:textSize">15sp</item>
    </style>
```
