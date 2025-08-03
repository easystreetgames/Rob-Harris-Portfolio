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

## Kinetic Art: Skinny Red Rectangle

This script makes a rotating skinny red rectangle that shifts to black as it rotates.

```script
Rect(color=#ff0000 w=5 h=500
  lerp(dur=9000 color=#000000 angle=180 out=drawDone ease=in-out)
)
```

## Kinetic Art: Shrinking Green Oval

This script makes a shrinking green oval that shifts to black as it shrinks.

```script
Oval(color=#00ff00 scale=12 
  lerp(scale=1 color=#000000 dur=8000 ease=in-out)
)
```

## Kinetic Art: Canvas Object

This script makes a Canvas object containing a rectangle and an oval.  A Canvas object is used to create an image source.  It is not displayed in the scene.  To view the image canvas in the scene, an Image object is created with its source set to the canvas object.  More than one Image object can use the canvas object as a source.

```script
// create image source (not displayed in scene)
Canvas(id=testCanvas w=900 h=900 color=#004
  // paint a rectangle onto the canvas
  Rect(color=#ff0000 w=5 h=500)

  // paint an oval onto the canvas
  Oval(color=#00ff00 scale=12)
)

// full-size image using canvas image source
Image(src=testCanvas w=900 h=900 x=100)

// small image using same image source
Image(src=testCanvas w=200 h=200 x=-500)
```

## Kinetic Art: Canvas With Rotating Rectangle

This script rotates the rectangle enclosed in the Canvas object.  Each frame as the rectangle rotates, it is drawn onto the canvas, resulting in a spirographic effect.  Once the rectangle is done rotating, the canvas object can be removed.  It can still be used as an image source by other objects.

```script
// create image source (not displayed in scene)
Canvas(id=testCanvas w=900 h=900 color=#004
  // paint a moving, dimming rectangle onto the canvas
  Rect(color=#ff0000 w=5 h=500
    lerp(dur=9000 color=#000000 angle=180 out=drawDone ease=in-out)
  )

  // the canvas can be removed from the scene once the animation is done
  // the final canvas image will remain available for use in the scene
  destroy(in=drawDone)
)

// full-size image using canvas image source
Image(src=testCanvas w=900 h=900)

// small image using same image source
Image(src=testCanvas w=100 h=100 x=-540)
```

## Kinetic Art: Canvas Visibility

This script toggles the image canvas visibility.  When the canvas is invisible, it does not draw its child objects.  This allows the animation to start and stop drawing.

```script
// create image source (not displayed in scene)
Canvas(id=testCanvas w=900 h=900 color=#004
  // paint a moving rectangle onto the canvas
  Rect(color=#ff0000 w=5 h=500
    lerp(dur=9000 color=#000000 angle=180 out=drawDone ease=in-out)
  )

  // the canvas can be removed from the scene once the animation is done
  // the final canvas image will remain available for use in the scene
  destroy(in=drawDone)
)

// full-size image using canvas image source
Image(src=testCanvas w=900 h=900)

// small image using same image source
Image(src=testCanvas w=100 h=100 x=-540)

Def(
  _(id=blink
    // make the canvas object visible
    update(target=testCanvas visible=true)

    // after a delay, make the canvas object invisible
    update(wait=30 target=testCanvas visible=false)

    // after a delay, loop by destroying this object and creating another
    make(wait=200 "blink" des)
  )
)
// start the blink logic
$blink()
```

## Kinetic Art: Canvas With Multiple Animating Objects

This script rotates the rectangle while shrinking the oval, producing an interesting swirling design.  The small image is rotated to produce a unique effect.

```script
// create image source (not displayed in scene)
Canvas(id=testCanvas w=900 h=900 color=#004
  // paint a moving rectangle onto the canvas
  Rect(color=#ff0000 w=5 h=500
    lerp(dur=9000 color=#000000 angle=180 out=drawDone ease=in-out)
  )

  // paint a shrinking ovalonto the canvas at the same time
  Oval(color=#00ff00 scale=12 
    lerp(scale=1 color=#000000 dur=8000 ease=in-out)
  )

  // the canvas can be removed from the scene once the animation is done
  // the final canvas image will remain available for use in the scene
  destroy(in=drawDone)
)

// full-size image using canvas image source
Image(src=testCanvas w=900 h=900)

// small image using same source
Image(src=testCanvas w=100 h=100 x=-540
  rotate(inf vel=10)
)
```

## Kinetic Art: Final Swirl Pattern

This script combines all elements from the proceeding samples to produce a final kinetic art piece.

```script
// create image source (not displayed in scene)
Canvas(id=testCanvas w=900 h=900 color=#004
  // paint a moving rectangle onto the canvas
  Rect(color=#ff0000 w=5 h=500
    lerp(dur=9000 color=#000000 angle=180 out=drawDone ease=in-out)
  )

  // paint a shrinking ovalonto the canvas at the same time
  Oval(color=#00ff00 scale=12 
    lerp(scale=1 color=#000000 dur=8000 ease=in-out)
  )

  // the canvas can be removed from the scene once the animation is done
  // the final canvas image will remain available for use in the scene
  destroy(in=drawDone)
)

// full-size image using canvas image source
Image(src=testCanvas w=900 h=900)

// small image using same source
Image(src=testCanvas w=150 h=150 x=-340 y=250
  rotate(inf vel=10)
)

// visibility toggle logic
Def(
  _(id=blink
    update(target=testCanvas visible=true)
    update(wait=30 target=testCanvas visible=false)
    make(wait=200 "blink" des)
  )
)
$blink()
```
