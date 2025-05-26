# Script Samples

Drop these into the sandbox for fun scripting effects.

## Spinning Letter
Add this one more than once for a fun effect.

```
// random letter and color
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  move(thrust=150 infinite)     // move based on current rotation
  rotate(vel=~-45<-90 infinite) // pick a random rotation velocity
)
```

Add some vertical randomness.
```
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  // quickly move at a random vertical speed
  move(dy=~100<1000 dur=200 block) // while blocking the other anims
  move(thrust=150 infinite)
  rotate(vel=~-45<-90 infinite)
)
```

Use a make anim to make 50 copies over time.
```
// make a container object to hold the make anim
_(y=100 x=0
  // quickly make 50 copies
  // use an in-line prototype to define the flying letter
  make(interval=100 count=50 script="
    Text(text=~Y,a,h,o,o,.,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
      move(dx=~100<1000 dy=~100<1000 dur=200 block)
      move(thrust=150 infinite)
      rotate(vel=~-45<-90 infinite)
    )" 
  )
)
```