# Style and Themes
Syltes and themes on Android allow you to separate the details of your app desing from the UI structure and behavior, similar to stylesheets in web design. 

A style is a collection of attributes that specify the appearnce for a single view. A style can specify attributes such as font color, font size, background color and much more. 

A theme is a collection of attributes that's applied to an entire app, activity, or view hierarchy not just an individual view. When you apply a theme, every view in the app or activity applies each of the theme's attributes that it supports. Themes can also apply styles to non-view elemetns, such as the status bar and window background. 

Styles and themes are declared in a style resource file in res/valus/ ususally named `styles.xml`

Themese and styles have many similarities, but theya re used for differen purposes. Themes and styles have the same basic strcuture a key value pair which maps attributes to resources. 

A styles specifies attributes for a particular type of view. For example, one style might specify a button's attributes. Every attrbiute you specify in a style is an attribute you could set in the layout file. By extracting all the attrbiutes to a style, it's easy to use and maintatin them across multiple widgets. 
A theme defines a collection of named resources which can be referecned by styles, layouts, widgets and so on. Themes assign semantic names, like colorPrimary, to Android resources. 

Styles and themes are meant to work toegther. For example, you might have a style that specifies that one part of a button should be `colorPriamry,` and another part should be colorSecondary. The actual definitions of those colors is provided in the theme. When the device goes into nightmode, your app can switch from its "light" theme to its "dark" theme, changing the values for all those resources names. you don't need to change the styles, since the styles are using the semantic names and not specific color definitions. 

## Create and apply a style
To createa new style or theme, open your project's `res/values/styles.xml` file for each style you want to create, follow these steps. 
1. Add a `<style>` element with a name that uniquely identifies the style. 
2. Add an `<item>` element for each style attribute you want to define. 
The name in each item specifies an attribute you would otherrwise use as an XML attribute in your layout. The value in the `<item>` element is the value ofr that attribute. 

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="GreenText" parent="TextAppearance.AppCompat">
        <item name="android:textColor">#00FF00</item>
    </style>
</resources>
```

You can apply styles to view as follows
```
<TextView
    style="@style/GreenText"
    ... />
```
Note: only the element to which you add the style atttribute receives those style attributes- any child view do not applly the styles. ifyou want child view to inherit styles, instead apply the style with the theme attrbiute. 

However, instead of applying a style to indvidual views, you'll usually apply styles as a theme for your entire app, activity or collection of views. 

## Extend and customize a style
When creating your own styles, you should always extend an exisitng style from the framework or support library so that you mantain compatibility witht platfrom UI styles. To extend a style, specify the style you want to extend with the parent atttribute. you can then override the inherited style atttributes and add new ones. 

```
<style name="GreenText" parent="@android:style/TextAppearance">
    <item name="android:textColor">#00FF00</item>
</style>
```

However, you should always inherit your core app styles from the Android Support Library. The styles in the support library provide compatibility and Android 4.0 and higher by optimizing each style for the UI attributes available in each version. The support library styles often have a name similar to the style from the platfrom, but with `AppCompat` included. 

To inherit styles from a library or your own project, declare the parent style name without the `@android:style/`part shown above. For example, the following example inherits text appearance styles form the support library. 
```
<style name="GreenText" parent="TextAppearance.AppCompat">
    <item name="android:textColor">#00FF00</item>
</style>
```

You can also inherit styles (expect those from the platform) by extending a style's name with a dot notation, instead of uisng the parent attribute. That is, prefix the name of your style with the name of the style you want to inherit, separated by a period. you should usually do this only when extending your own styles, not styles from other libraries. For example, the following style inherits all styles from the `GreenText` style above and then increases the text size. 

```
<style name="GreenText.Large">
    <item name="android:textSize">22dp</item>
</style>
```

You can conintue inherting styles like this as many times as you'd like  by chaining on more names. 
Note: If you use the dot notation to extend a style, and you also include the parent attrbite, then the parent styles override any styles inheritted through the dot notation. 

To find which attributes you can delcare with an `<item>` tab, refere to the "XML attributes" table in the various class references. All views support `XML attributes form the base View class`, and manyviews add theier own special attributes. For example, the `TextView XML attrbiutes`includes the `android:inputType` attribute that you can apply to a text view that receives input, such as an `EditText` widget. 


## Apply a style as a Theme
You can create a theme the same way you create styles The difference is how you apply it: instead of applying a style with the style attribute on a view, you apply a theme with the `android:theme` attribute on either the `<application>` tag or an `<activity>` tag in the `AndroidManifest.xml`

For example, here's how to apply the Android Support Library's material design "dark" them to the whol app
```
<manifest ... >
  <application android:theme="@style/Theme.AppCompat" ...>
  </application>
</manifest>
```
Ahd here's how to apply the light them to just one activity
```
<manifest ... >
    <application ... >
        <activity android:theme="@style/Theme.AppCompat.Light" ... >
        </activity>
    </application>
</manifest>
```

Not every view in the app or activity applies the styles defined in the given theme. If a veiw supports only some of the attribute declared in the style, then it applies only those attributes and ignores the one it does not support. 

Beginning with Android 5.0 and Android Suppport Library v22.1, you can also specify the `android:theme` attribute to a view in your layout file. This modifies the theme for that view and any child views, which is useful for altering theme color paettes in a specific portion of your interface. 

The previous examples show how to apply a theme such as `Theme.AppCopmat` that's suppplied by the Android Support Library. but you'll usually want to customize the theme to fit your app's brand. The best way to do so is to extend these styles from the support library and overrid e some of the attributes, as descirbed in the next section . 

