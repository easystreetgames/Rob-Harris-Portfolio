# Script Samples

Drop these into the sandbox for fun scripting effects.

## Spinning Letter
Add this one more than once for a fun effect.

```
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  move(thrust=150 inf)
  rotate(v=~-45<-90 inf)
)
```

Add some vertical randomness.
```
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  move(dy=~100<1000 dur=200 b)
  move(thrust=150 inf)
  rotate(v=~-45<-90 inf)
)
```

Use a make anim to make 50 copies over time.
```
_(y=100 x=0
  make(interval=50 count=50 script="
    Text(text=~Y,a,h,o,o,.,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
      move(dy=~100<1000 dur=200 b)
      move(thrust=150 inf)
      rotate(v=~-45<-90 inf)
    )" 
  )
)
```