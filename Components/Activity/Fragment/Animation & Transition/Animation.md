# Fragment Navigation Animation
You can specify custom animation for entering and exiting fragments. 

## 1) Enter and Exit Animation
Enter is when the fragment enters the screen and exit is when the fragment leaves the screen. Create an XML file and save it to res/anim

exit
```
<!-- res/anim/fade_out.xml -->
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="1"
    android:toAlpha="0" />
```
enter
```
<!-- res/anim/slide_in.xml -->
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="100%"
    android:toXDelta="0%" />
```

## 2) PopEnter and PopBack stack
To specify animation for the enter and exit effects that are run when popping the back stack(Back button). Create a file in res/anim dirctory. These are called popEnter and PopExit animations. For example, when a user pops back to a previous screen.
```
<!-- res/anim/slide_out.xml -->
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="0%"
    android:toXDelta="100%" />
```

```
<!-- res/anim/fade_in.xml -->
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="0"
    android:toAlpha="1" />
```

## 3) FragmentTrasaction
Once youv'e define your animation, use them by calling fragmentTransaction.setCustomAnimation(), passing in the animation resources by their resource ID.
