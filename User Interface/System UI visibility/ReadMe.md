# Control the systme UI visisbility 

The system bars are screen areas dedicated to the display of notifications, communication of device status, and device navigation. Typically the sytem bars are displayed concurrently with our app. Apps that dispplay immersive content, such as movies or images, can temporarily dim the stystme bar iconss for a less distracting experience, or temporarily hid the bars for a fully immersive experience. 


WindowsInsetControler
INterface to control windows that generate insets. 

Use SystemBarHeavior to set the touch behavior of the system bar. 


hide Statbar
```
val controller = requireActivity().window.insetsController
controller?.hide(WindowInsets.Type.statusBars())
```

Hide Naivgation
```
val controller = requireActivity().window.insetsController
controller?.systemBarsBehavior = WindowInsetsController.BEHAVIOR_SHOW_BARS_BY_SWIPE
controller?.hide(WindowInsets.Type.navigationBars())
```


Full screen leanback,  show bars on touch
```
val controller = requireActivity().window.insetsController
controller?.systemBarsBehavior = WindowInsetsController.BEHAVIOR_SHOW_BARS_BY_TOUCH
controller?.hide(WindowInsets.Type.systemBars())
```

Full screen Immersive, show system bars on swipe.
```
val controller = requireActivity().window.insetsController
controller?.systemBarsBehavior = WindowInsetsController.BEHAVIOR_SHOW_BARS_BY_SWIPE
controller?.hide(WindowInsets.Type.systemBars())

```

Fuscreen Immersive stick, show transparent system bars when a swipe occurs.
