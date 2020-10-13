# How setup MaterialcardView

## 1) Dependencies
```
    implementation 'com.google.android.material:material:1.2.1'
```

## 2) Set app style
Set the style to be child them of MaterialCOmponents
```
    <style name="AppTheme" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
```

## 3) add MaterialCarview to XML
```
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="300dp"
    android:layout_height="wrap_content"
    app:cardBackgroundColor="#D5B4B4"
    app:cardCornerRadius="10dp"
    app:strokeColor="#eb8034"
    app:strokeWidth="10dp">

```
