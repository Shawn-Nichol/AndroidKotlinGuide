## AnimatorSet
Animator set lets you bundle animations together, so that you can specify whether to start animations simultaneously, sequentially or after a specified delay, you can also nest animatoset objects within each other. 


Avoid circular dependcies as will prevent the animation from running
ex start before animation a2, a2 before a3,  and a3 before a1

ex. Chain animation together. 

```
val bouncer = AnimaotrSet().apply {
    play(bounceAnim).before(squashAnim1)
    play(squashAnim1).with(squashAnim2)
    play(squashAnim1).with(stretchAnim1)
    play(squashAnim1).with(stretchAnim2)
    play(bounceBackAnim).after(stretchAnim2)
}

val fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f).apply {
    duration = 250
}
AnimatorSet().apply {
    play(bouncer).before(fadeAnim)
    start()
}
```

ex. 
```
Animator.set().apply {
  play(Anim1)
    .before(anim2)
    .with(anim3)
    .with(anim4)
    .after(anim5)
    strat()
}
```


ex play sequentially
```
  val set: AnimatorSet().apply {
    playTogether(anim1, anim2)
    duration = 2000
  }
  set.start()
```
