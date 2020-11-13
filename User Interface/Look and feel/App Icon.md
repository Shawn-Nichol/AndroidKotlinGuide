Adaptive icons
Android 8.0 introduces adaptive launcher icons, which can display a vriety of shapes across differen device models. For example, an adaptive launcher icon can display a circular shape on one oEM device, and dispaly a aquaircle on another device. Each deice OEM provides a mask wich thte system then uses to render all adaptive icons with thte same shape. Adaptive launcher icons also used in shortcuts, the settings app, sharing dialogs, and the overview screen. 

You can control the look of your adaptive launcher icon by definning 2layouers, consisting of a background a foreground. You must provide icon layers as draables without masks or background shadows around the outline of the icon

Layers must be sized at 108 X 108 dp
The inner 72 X 72 dp of the icon appears within the masked viewport. 
The system reserves the outer 18 dp on each of the 4 sides to create interesting visual effects such as a parallax or pulsing. 


## Creating adaptive icons in XML
To add an adaptive icon to an app using XML, begin by updating the `android:icon` attribute in your app manifest to specify a drawable resource. you can also define an icon drawable resource using the `android:roundIcon` attrbiute. You must only use the `android:roundIcon` attrbiute if you require a different icon asset for circular masks, if  for example the branding of your lgoo relies on a circular shape. The following code snippet illustrates both of these attrbiutes

```
<application
  android:icon"mipmap/ic_launcher"
  android:roundIcon="@mipmap/ic_launcher-round
  ...>

```

Next you must create alternative drawable resources in your app for use with Android 8.0 API level 26. You can then ues the `<adaptive-icon>` element to define the foregroudn and background layer drawables for your icons. The `<foreground>` and `<background>` inner elements both support the android: drawable attribute. 
```
<?xml version="1.0" encoding="utf-8"?>
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@drawable/ic_launcher_background" />
    <foreground android:drawable="@drawable/ic_launcher_foreground" />
</adaptive-icon>
```

You can also define the background and foreground drawables as elements by enclosing them in `<foreground>` and `<background>` elements. 

If you want to apply the same mask and visual effect to your shortcuts as regular adaptive launcher icons, use one of the following techniques
- For static shortcuts, use the `<adaptive-icon>` element
- For dynamic shortcuts, call the `createWithAdaptiveBitmap()` method when you create them. 

