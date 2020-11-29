# Control the systme UI visisbility 

The systme bars are screen areas dedicated to the display of notifications, communication of device status, and device navigation. Typically the sytem bars are displayed concurrently with our app. Apps that dispplay immersive content, such as movies or images, can temporarily dim the stystme bar iconss for a less distracting experience, or temporarily hid the bars for a fully immersive experience. 

## Dimm System Bars
When you sue this appraocach, the content doesn't resize, but the icons in the system bars visually reced. as soon as the user touches either the status bar or the navigation bar area of the screen, both bars beocme fullly visible. The advantage of this approach is that the bars are sstill preent but their details are boscured, thus creating an immersive experience withot sacrificing easy access to the bars. 

## Dim the status and Navigation Bars. 
```
// This example uses decor view, but you can use any visible view.
activity?.window?.decorView?.apply {
    systemUiVisibility = View.SYSTEM_UI_FLAG_LOW_PROFILE
}
```
As soon as the user touches the status or navigation bar, the flag is cleared, causing the bars to be undimmed.Once the flag has been cleared, your app needs to reset it if you want to dim the bars again. 

To programimatically clear flags sest with setSystemvisibility(),
```
activity?.window?.decorView?.apply {
    // Calling setSystemUiVisibility() with a value of 0 clears
    // all flags.
    systemUiVisibility = 0
}
```
