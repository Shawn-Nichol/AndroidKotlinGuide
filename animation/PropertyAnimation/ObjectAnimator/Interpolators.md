# Interpolators
An interpolator define how specific values in animation are calculated as a function of time. For exapmle you can spedify animation to happen linearly across the whole animation, meaning the animation moves evenly the entire time, or you can specify animation to use non-linear time, for example using acceleration or deceleration at the beginnning or end of the animtion. 

Interpolators in the animation system receive a fraction from animators that represent the elapsed time of the animation. Interpolators modify this fraction to coincide with the type of animation that it aims to provide. The Android system provides a set of common interpolators in teh android.view.animation package. If none of these suit your needs, you can implement the TimeInterpolator interface anc create your own. 

As an example how the dfault interpolator AccelerateDecelerateInterpolator and the LinearInterplator calculate interpolated fractions are compared below. The linearINterpolator has no effect on the elapsed fraction The AccelerateDecelerateInterpolator accelerates into the animation and decelerates out of it. 

ex AccelerateDecelerateInterpolator
```
override fun getInterpolation(input: Float): Float = (Math.cos((input + 1) * Math.PI) / 2.0f).toFloat() + 0.5f
```

ex LinearInterpolator
```
override fun getInterpolation(input: Float): Float = input
```

