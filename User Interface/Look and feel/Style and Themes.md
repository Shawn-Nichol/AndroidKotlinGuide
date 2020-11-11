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

## Style Hierarchy
Android provides a variety of ways to set attrbiutes throughout your Android app. For examlple, you can set attributes directly in a layout, you can apply a style to a view, you can apply a theme to a layout, and you can even set attrbiutes programmatically. 

When choosing how to style your app, be mindful of Android's style heirarchy. In general, you should use themes and styles as much as possible for sonsistency. if you've specified the same attrbiutes in muyltiple places, the list below deteremines which attributes are ultimately applied. The list is ordered from highest precedence t loweset:

1. Apply character or paragraphy-level styleing via text spans to `TextView-derived classes.
2. Applying attributes progammactically
3. Applying individual attrbiutes directly to a view
4. Apply a style to a view
5. Default styleing
6. Apllying a theme to a collection of views, an activity, or your entire app
7. Applying certain View-specific styling, such as setting a TextAppearance on a TextView. 


## TextAppearance
One limitation with styles is that you can apply only one style to a view. In a `TextView`, however you can also specify a `TextAppearance` attrbiute which functions similarly to a style, as shown in the following. 

```
<TextView
    ...
    android:textAppearance="@android:style/TextAppearance.Material.Headline"
    android:text="This text is styled via textAppearance!" />
```

`TextAppearance` allows you to define text-specific styling while leaving a `View's` stle available for other uses. Note, however that if you define any text attrbiutes directly on the `View` or in a style, those values would override the `TextAppearance` values. 

`TextAppearance` supports s subset of styling attrbiutes that `TextView` offers. For the full attrbiute list, see `TextAppearance`.

Some common `TextView` atttributes not included are `lineHeight[Multiplier|Extra]`, `lines`, `breakStrategy`, and `hyphenationFreqnency`. `TextAppearance` works at the character level and not he paragraph level, so attributes that affect the entire layout are not supported. 

## Customize the defual theme. 
When you create a project with Andoid studio, it applies a material design theme t o your app by default, as defined in your project's `styles.xml` file. This `AppTheme` style extends a theme from the support library and includes overrides for color attrbiutes that are used by tkey UI elements, such as the app bar and `FAB`. So you can quickly customize your app's color design by updaitn the provides colors. 

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <!-- Customize your theme here. -->
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
</style>
```

Notice that the style values are actually reference to other color resources, defined in the project's `res/values/colors.xml` filess. So that's the file you should edit to change the colors. But before you start changing these colors, preview your colors with the `Material Color Tool`. This tool helps you pick colors fomr the material palette and preview how they'll look in an app. 

Once you know your colors, update the values in `res/values/colors.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--   color for the app bar and other primary UI elements -->
    <color name="colorPrimary">#3F51B5</color>

    <!--   a darker variant of the primary color, used for
           the status bar (on Android 5.0+) and contextual app bars -->
    <color name="colorPrimaryDark">#303F9F</color>

    <!--   a secondary color for controls like checkboxes and text fields -->
    <color name="colorAccent">#FF4081</color>
</resources>
```

And then you can override what ever other styles you want. For exmaple, you can change the activity background color as follows. 
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    ...
    <item name="android:windowBackground">@color/activityBackground</item>
</style>
```

For a list of attrbiutes you can use in your theme, see the table of attrbitues at `R.styleable.Theme` and when adding styles for the views in your layout, you can also find attrbiutes by looking at the "XML attrbiutes" table in the view class references. For example, all views support `XML attrbiutes from the base view class.`

Most atttributes are applied to specific types of views, and some apply to all views. However, some theme attrbiutes listed at `R.styleable.Theme` apply to the activity window, no the views in the layout. For example,  windowBackground changes the window background and windowEnterTransition defines a transition animation to use when the aticity starts

The Android Support Library also provies other attributes you can use to customize your theme extend from `Theme.ApCompat`(such as the colorPrimary attribute shown above). Theses are best viewed in teh library's attrs.xml file. 

There are also differen themes available from the suport library that you might want to extned instead of the ones shonwn above. The best place to see the available themes is the library's themes.xml file. 

## Add Version-specific styles
If a new version of Android adds theme attrbiutes that you want to use, you can add them to your theme while still being compatible with old versions. All you need is another styles.xml file saved in a values directory that includes the resource version qualifier. 
```
res/values/styles.xml        # themes for all versions
res/values-v21/styles.xml    # themes for API level 21+ only
```

Becuase the styles in the `values/styles.xml` files are available for all version, your themes in `values-v21//styles.xml` can inherit them. As such, you can avoid duplicating styles by beginningwith a "base" theme and then extending it in your version specific styles. 

For exmple, to declare window transitions for Android 5.0 and higher you need to use some new attributes. So your base theme in res/values/styles. xml could look like this. 

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
Then add the  version-specific styles in `res/values-v21/styles.xml`

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

Now you can apply `AppTheme` in your manifest file and the stsytem selects the styles avaialbel for each system version. 

## Customize widget styles 
Every widget in the framework and support library has a default style. For example, when you style your app using a them from the support library, an instance of  `Button` is styled using the `Widget.AppCompat.Button` style,. If you'd like to apply a different widget style to a button, then you can do so with the style atttrbiute in your layout file. For example, the following applies the library's borderless button style. 

```
<Button
    style="@style/Widget.AppCompat.Button.Borderless"
    ... />
```

And if you want to apply this style to all buttons, you can declare it in your theme's buttonStyle as follows

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="buttonStyle">@style/Widget.AppCompat.Button.Borderless</item>
    ...
</style>
```

You can also extend widget styles, just like extending any other tstyle, and then apply your custom widget style in your layout or in your theme. 

