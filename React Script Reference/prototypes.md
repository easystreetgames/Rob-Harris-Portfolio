# Prototypes
A prototype is a script that is used to create one or more copies of an object.  A prototype can be defined as a global variable or it can be an in-line definition.

There are multiple ways to make protoype copies either when a script is paresed or later using an anim.  The **Proto()** command and **$** notation create copies at parse-time.  The **anim** anim and **make** anim create copies after the scene has started.

## Proto Command
Prototype copies can be created when a script is parsed using the Proto() command.  This command takes either a global variable ID or a quoted script and makes an object that is added to the current scene.

```
// define the prototype
Define(
    Text(id=hello text=Hello size=50 x=-200 
        move(vx=~50<150 vy=~-50<50 dur=~1000<5000 destroy)
    )
    Text(id=hola text=Hola size=50 x=200 
        move(vx=~50<150 vy=~-50<50 dur=~1000<5000 destroy)
    )
)

// use the Proto command to create one or more copies of a prototype
// limits: can't delay the creation or repeat at intervals
Proto(count=2 script=hello)

// use quotes as a shortcut for text=
Proto(count=3 "hello")

// an in-line prototype definition
// useful for creating multiple copies
Proto(count=5 "Oval(move(dy=~100<300 des))")
```
## $ Notation
The $ notation is very similar to the Proto() command, but it also makes it easy to override prototype properties.
```
// $ proto notation
// same limits as Proto() command
// advantage: easy to override properties
$hola(y=100 color=yellow)
```
### * Notation
Multiple copies can be made by using the * notation with the $ notation.
```
// * count notation
// used to make multiple copies
$hello*3(y=-100 color=red)
```
## The Basic Anim
The **anim** anim can be used to create a prototype copy when an anim is completed.  Since all anims inherit the properties of the basic anim, this can be quite powerful.
```

// define the prototype
Define(
  Text(id=hello text=Hello size=50 color=#2e5
    move(vx=~50<150 vy=~-50<50 dur=~1000<5000 destroy)
  )
)

// use a basic anim to create a prototype upon completion
// advantage: all anims inherit this ability
_(
  // reference the prototype by ID
  anim(wait=1000 make=hello)

  // in-line prototype definition
  anim(wait=3000 make=
    "Rect(color=yellow
      rotate(inf)
      move(dx=100 dur=5000 des)
    )"
  )

  // in-line prototype definition 
  // uses the $ notation to reference the prototype by ID
  // (allows multiple copies and property overrides)
  anim(wait=2000 make="$hello*4(y=-150 color=orange)")
)
```
### Inheriting Basic Properties
```
// example: all anims inherit the make property
// start hidden below the view
Text("Surprise!" y=500
  // wait 4 secs, fly into view, then make a flying circle 
  lerp(wait=4000 y=0 make="Circle(color=red move(dy=-100 destroy))")
)

Rect(y=500
  move(wait=5000 dx=75 dy=-100 dur=5000 make="$hello*9(color=~red,yellow,orange)")
)
```
## The Make Anim
The **make** anim provides a way to create prototype copies over time, like for particle effects.
```
// define the prototype
Define(
    Text(id=hello text=Hello size=50 x=-200 
        move(vx=~50<150 vy=~-50<50 dur=~1000<5000 destroy)
    )
)

// make anim
// advantage: can create copies over time
_(
    // shortcut notation
    make(wait=1000 'hello')
    make(wait=2000 '$hello(y=250 color=black)')

    make(wait=3000 count=3 interval=1000 "$hello*9(y=-200 color=purple size=20)")

    // make 20 copies of the prototype over time
    make(wait=4000 script=hello count=20 interval=500)

    // inline prototype using shortcut notation
    make(wait=5000 count=10 
        "Rect(x=-200 y=200
            rotate(inf)
            move(dx=100 dur=5000 des)
        )"
    )
)
```