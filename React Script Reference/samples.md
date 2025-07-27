# Script Samples

Drop these into the sandbox for fun scripting effects.

## Spinning Letter

Add this one more than once for a fun effect.

```script
// random letter and color
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  move(thrust=150 infinite)     // move based on current rotation
  rotate(vel=~-45<-90 infinite) // pick a random rotation velocity
)
```

Add some vertical randomness.

```script
Text(text=~Y,a,h,o,o,!,.,*,.,.,. color=~#f2f,#f4f,#f6f,#f8f,#faf,#f0f
  // quickly move at a random vertical speed
  move(dy=~100<1000 dur=200 block) // while blocking the other anims
  move(thrust=150 infinite)
  rotate(vel=~-45<-90 infinite)
)
```

Use a make anim to make 50 copies over time.

```script
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

## Spinning Ovals

This script makes a spinning oval.

```script
Def(
  canvas.bg=#251

  Oval(id=fx1.rectOne w=300 color=#77c stroke=#000)

  fx1.spinner.x=-300
  fx1.spinner.dx=100
  fx1.ypos=0
  
  $fx1.rectOne(id=fx1.spin x=$fx1.spinner.x y=$fx1.ypos
    rotate(inf)
    move(dx=$fx1.spinner.dx dur=20000 des)
  )
)

$fx1.spin()

```

This script makes a bunch of spinning ovals in an interesting pattern.

```script
Def(
  canvas.bg=#251

  Oval(id=fx1.rectOne w=300 color=#77c stroke=#000)

  fx1.spinner.x=-300
  fx1.spinner.dx=100
  fx1.ypos=0
  
  $fx1.rectOne(id=fx1.spin x=$fx1.spinner.x y=$fx1.ypos
    rotate(inf)
    move(dx=$fx1.spinner.dx dur=20000 des)
  )
  
  _(id=fx1.spinner
    make(wait=50 count=20 interval=500 
      "@(set(fx1.spinner.x=-700 fx1.spinner.dx=100 make=fx1.spin))"
    )
    make(wait=50 count=20 interval=500 
      "@(set(fx1.spinner.x=700 fx1.spinner.dx=-100 make=fx1.spin))"
    )
  )
)

$fx1.spinner()

```

Here is another script that makes a bunch of spinning ovals in an interesting pattern.

```script
Def(
  canvas.bg=#251

  Oval(id=fx1.rectOne w=300 color=#77c stroke=#000)

  fx1.spinner.x=-300
  fx1.spinner.dx=100
  
  $fx1.rectOne(id=fx1.spin x=$fx1.spinner.x y=$fx1.ypos
    rotate(inf)
    move(dx=$fx1.spinner.dx dur=20000 des)
  )
  
  _(id=fx1.spinner
    make(wait=50 count=20 interval=500 
      "@(set(fx1.spinner.x=-700 fx1.spinner.dx=100 make=fx1.spin))"
    )
    make(wait=50 count=20 interval=500 
      "@(set(fx1.spinner.x=700 fx1.spinner.dx=-100 make=fx1.spin))"
    )
  )
)
/*
$fx1.spinner()
*/
_(
  set(fx1.ypos=300 make=fx1.spinner)
  set(wait=5000 fx1.ypos=0 make=fx1.spinner b)
  set(wait=5000 fx1.ypos=-400 make=fx1.spinner b)
)

```

## Prototypes: Created With Basic Anim

This script demonstrates how to use a basic anim to create a prototype copy that has no parent object.

```script
Def(
  Text(id=textObj text=Hello y=100
    // move to the right at random speed, then die
    move(wait=1000 vx=~50<150 dur=9000 destroy)
  )
)

// Create copies with no parent
_(id=creator
  // use proto definition
  anim(wait=1000 make=textObj)

  // use proto definition with additional properties
  anim(wait=4000 make="$textObj(text='no parent')")

  // use in-line definition
  anim(wait=7000 make="Rect(y=100 move(dx=100 dur=9000 des))")
)
```

This script demonstrates how to use a basic anim to create a prototype copy that has no parent object, but uses its creator's transform (position and rotation).  An oval has been added to the creator object to show its current rotation and position.

```script
Def(
  Text(id=textObj text=Hello y=100
    move(wait=1000 vx=~50<150 dur=9000 destroy)
  )
)

_(id=creator y=-100
  oval(w=100 h=5)
  rotate(inf)

  // Create instance with no parent that uses creator’s transform
  anim(wait=500 make=@textObj)
  anim(wait=3500 make="@$textObj('no parent, just transform')")
  anim(wait=6500 make="@Rect(y=100 move(dx=100 dur=9000 des))")
)
```

This script demonstrates how to use a basic anim to create a prototype copy that has a parent object.  An oval has been added to the creator object to show its current rotation and position.

```script
Def(
  Text(id=textObj text=Hello y=100
    move(wait=1000 vx=~50<150 dur=9000 destroy)
  )
)

_(id=creator y=-100 rot=45
  oval(w=100 h=5)
  rotate(inf)

  //Create instance that is a child of the creator
  anim(wait=500 make='@(textObj)')
  anim(wait=3500 make="@($textObj('child'))")
  anim(wait=6500 make="@(Rect(y=100 move(dx=100 dur=9000 des)))")
)
```

## Prototypes: Created With Make Anim

This script demonstrates how to use the make anim to create prototype copies that have no parent object.

```script
Def(
  Text(id=textObj text=Hello y=100
    move(wait=1000 vx=~50<150 dur=9000 destroy)
  )
)

_(id=creator
  // Create instance with no parent
  make(wait=1000 'textObj' inf int=7000)
  make(wait=4000 "$textObj(text='no parent')" inf int=7000)
  make(wait=7000 'Rect(y=100 move(dx=100 dur=9000 des))' inf int=7000)
)
```

This script demonstrates how to use the make anim to create a prototype copy that has no parent object, but uses its creator's transform (position and rotation).  An oval has been added to the creator object to show its current rotation and position.

```script
Def(
  Text(id=textObj text=Hello y=100
    move(wait=1000 vx=~50<150 dur=9000 destroy)
  )
)

_(id=creator
  oval(w=100 h=5)
  rotate(inf)
  
  // Create instance with no parent
  make(wait=500 '@textObj' inf int=7000)
  make(wait=3500 "@$textObj('no parent, just transform')" inf int=7000)
  make(wait=6500 '@Rect(y=100 move(dx=100 dur=9000 des))' inf int=7000)
)
```

This script demonstrates how to use the make anim to create a prototype copy that has a parent object.  An oval has been added to the creator object to show its current rotation and position.

```script
Def(
  Text(id=textObj text=Hello y=100
    move(wait=1000 vx=~50<150 dur=4000 destroy)
  )
)

_(id=creator
  oval(w=100 h=5)
  rotate(inf)
  
  // Create instance with no parent
  make(wait=500 '@(textObj)' inf int=7000)
  make(wait=3500 "@($textObj('child'))" inf int=7000)
  make(wait=6500 '@(Rect(y=100 move(dx=100 dur=4000 des)))' inf int=7000)
)
```

This script creates prototype copies that are children of the container.

```script
Define(
  Text(id=textObj text="Come together!" size=30 x=~-200<200 y=~-200<200 
    lerp(x=0 y=0 angle=0 dur=10000)
    lerp(wait=9500 alpha=0 des)
  )
)

Cont(
  rotate(inf vel=10)
  Rect(w=400 h=10 alpha=.2)
  make(wait=100 "@(textObj)" count=10 interval=1000)
)
```

This script creates prototype copies that use their creator's transform (position and rotation) but are not children of the container.

```script
Define(
  Text(id=textObj text="Come together!" size=30 x=~-200<200 y=~-200<200 
    lerp(x=0 y=0 angle=0 dur=10000)
    lerp(wait=9500 alpha=0 des)
  )
)

Cont(
  rotate(inf vel=10)
  Rect(w=400 h=10 alpha=.2)
  make(wait=100 "@textObj" count=10 interval=1000)
)
```
