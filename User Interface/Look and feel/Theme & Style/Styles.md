# Styles
Styles and themes on Android allow you to separate the detials of your app desin from the UI strcuture and behavior. 

A style is a collection of attrbiutes that specify the appearance for a single view. A Style can specify attrbiutes such as font color, font size, background color and much more. 

Styles are declared in a style resource file in `res/values` usually named `styles.xml`

A styles specifies attrbiutes for a particular type of view For example, one style might specify a button's attrbiutes. Every attribute you specify in a style is an attrbiute you could set in the layout file. By extracting all the attrbiutes to a style, it's easy to use and maintain them across multiple widgets. 

## Create and apply a Style
To createa new style or theme, open your project's `res/values/styles.xml` file for each style you want to create.

1. Add a `<style>` element with a name that uniquely identifies the style. 
2. Add an `<item>`element for each style attribute you want to define. The name in each item specfies an attribute you would otherwise use as an XML attrbiute in your layout. The value in the `<item>` element is the value of that attribute. 

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="GreenText" parent="TextAppearance.AppCompat">
        <item name="android:textColor">#00FF00</item>
    </style>
</resources>
```

You can apply styles to a view.
```
<TextView
    style="@style/GreenText"
    ... />
```

Note: only the element to which you add the style attribute receives those style attrbiutes any child view do not applly the styles. If you want child view to inherit styles, instead apply the style with the theme attrbiute. 

## extend and customize a style
Wehn creating your own styles, you should always extend an existing style from the framework or support library so that you maintain compatibility with the platform UI styles. To extend a style, specify the style you want to extend with the parent attribute. You can then override the inherited style attrbiutes and add new ones. 

```
<style name="GreenText" parent="@android:style/TextAppearance">
    <item name="android:textColor">#00FF00</item>
</style>
```

However, you should always inherit your core app styles from the Android support library. THe styles in teh support library provide compatibility and Android 4.0 and higher by optimizing each style for the UI attrbiutes available in each version. The support library styles often have a name similar to the style from the platform, but with `AppCompt` included. 

To inherit styles from a library or you own project, declare the parent style name without the `@android:style/`. For example the following example inherits text appearance stylse from the support library. 

```
<style name="GreenText" parent="TextAppearance.AppCompat">
    <item name="android:textColor">#00FF00</item>
</style>
```
You can also inherit styles (expect those from the platform) by sextending a style's name with a dot notation, instead of using the parent atttribute. That is, prefix the name of our style with the name of the style you want ot inherit, separated by a period. You should usually do this only when extending yoyyour own styles, not styles from other libraries. For example, the folloiwng style inherits all styles from the GreenText style. 

```
<style name="GreenText.Large">
    <item name="android:textSize">22dp</item>
</style>
```

You can continue inherting styles like this as many times as you'd like by chaining on more names. Note: if you use the dot notation to extend a style, you also includ the parent attribute, then the parent styles override any styles inheritted through the dot notation. 

To find which attributes you can delcare with an `<item>` tab,refer to the "XML attributes" table in the various class references. All views support `XML attrbiutes from the base View class`, and many views add their own special attributes. For example, the `TextView XML` attributes includes the `android:inputType` attrbiute that you can apply to a text view that receives tinput, such as an `EditText` widget. 

## Style Hierarchy
Android provides a variety of ways to set attributes throught your Android app. For example, you can set attributes directly in a layout, you can apply a style to a view, you can apply a theme to a layout, and you can even set attrbiutes programmatically. 

When choosing how to style your app, be mindful of Android's style heirarchy. In gerenal, you should use themes and styles as much as possible for consistency. If you've specifed the same attrbiutes in multiple places. the list below determines which attributes are ultimately applied. The list is ordered form hiest precendence to lowest. 

1. Apply character or paragraph-level styling via text spans to `TextView-derived classes`
2. Apply attributes programactically. 
3. Apply individual attributes directly toa view. 
4. Apply a style to a view. 
5. Default styling
6. Applying a theme to a collection of views, an activity, or  your entire app. 
7. Applying certain View-specific syling, such as setting a TextAppearance on a TextView. 


## Text Appearance 
One limitation with styles is that you can apply only one style to a view. In a `TextView`, however you can also specify a `TextAppearance` attrbiute which functions similarly to a style. 

```
<TextView
    ...
    android:textAppearance="@android:style/TextAppearance.Material.Headline"
    android:text="This text is styled via textAppearance!" />
```

`TextAppearance` allows you to define text-specific styling while leaving a `View's` style available for other uses. Note, however that if you define any text attrbiutes directly on the `View or in a style, those values would override the `TextAppearance` values. 

`TextAppearance` supports subset of styling attributes that `TextView` offers. For the full attrbiute list, see `TextAppearance`.

Some common `TextView` attrbiutes not included are `lineHeight[Multiplier|Extra]`, lines, `breakStrategy, and `hyphenationFreqency. `TextAppearance` works at the character level and not the paragraph level, so attributes that affec the entire layout are not supported. 

## Add Version-sepecific styles
If a new version of Android adds theem attrbiutes that you want to use, you can add them to your theme  while still being compatible with old veresions. All you need i santoher styles.xml file saved in a values directory that includes the resource version qualififier

```
res/values/styles.xml        # themes for all versions
res/values-v21/styles.xml    # themes for API level 21+ only
```

Becuase the styles in the `values/styles.xml files are aviable fo all version, the themes in `values-v21/styles.xml` can inherit tham. As such, you can avoid duplicating styles by begining with a "base" theme and then extending it in your version specific styles. 

For example, to declare window transitions for Android 5.0 and higher you need to use some new attrbiutes. So your base theme in `res/values/styles.xml`

```
<resources>
    <!-- base set of styles that apply to all versions -->
    <style name="BaseAppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="colorPrimary">@color/primaryColor</item>
        <item name="colorPrimaryDark">@color/primaryTextColor</item>
        <item name="colorAccent">@color/secondaryColor</item>
    </style>

    <!-- declare the theme name that's actually applied in the manifest file -->
    <style name="AppTheme" parent="BaseAppTheme" />
</resources>
```

Then add the version-specific styles in `res/values-v21/styles.xml`
```
<resources>
    <!-- extend the base theme to add styles available only with API level 21+ -->
    <style name="AppTheme" parent="BaseAppTheme">
        <item name="android:windowActivityTransitions">true</item>
        <item name="android:windowEnterTransition">@android:transition/slide_right</item>
        <item name="android:windowExitTransition">@android:transition/slide_left</item>
    </style>
</resources
```
Now you can apply `AppTheme` in your manifest file and the styem selects the styles available for each ssytem version. 

## Customize widget styles
Every widget in the framework and support library has a default style. For example, when you style your app using a theme from the support library, an instance of `Button` is styled using the `Widget.Appcompat.Button` style. If you'd like to apply a different widget style to a button, then you can do so with the style attribute in your layout file. For exmaple, the following applies the library's borderless button style. 

```
<Button
    style="@style/Widget.AppCompat.Button.Borderless"
    ... />
```

And if you want to apply this style to all buttons, you can delcare it in your theme's button style as follows
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="buttonStyle">@style/Widget.AppCompat.Button.Borderless</item>
    ...
</style>
```

You can also extend widget styles, just like extending any other style, and then apply your custom widget style in your layout or in your theme. 
