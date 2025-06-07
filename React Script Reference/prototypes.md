# Prototypes

A prototype is a script that is used to create one or more copies of an object.  A prototype can be defined as a global variable or it can be an in-line definition.

There are multiple ways to make protoype copies: when a script is parsed or later using an anim.  The [Proto()](./objects.md#prototypes) command and [$ notation](./objects.md#prototype-id) create copies at parse-time.  The [anim's](./anims.md#anim--listen) make property and the [make](./anims.md#make) and [if](/anims.md#if--compare) anims create copies after the scene has started.

## Proto Command

Prototype copies can be created when a script is parsed using the [Proto()](./objects.md#prototypes) command.  This command takes either a global variable ID or a quoted script and makes an object that is added to the current scene.

```script
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

// use quotes as a shortcut for script=
Proto(count=3 "hello")

// an in-line prototype definition
// useful for creating multiple copies
Proto(count=5 "Oval(move(dy=~100<300 des))")

```

### Randomness

When using random notation, it is important to understand that the random value is resolved when a script is parsed.  Prototypes work well with randomness because the prototype definition retains the random notation, which is not resolved until the prototype definition is parsed to make one or more copies.  Each copy will have unique random values.

## $ Notation

The [$ notation](./objects.md#prototype-id) is very similar to the Proto() command, but it also makes it easy to override prototype properties.

```script
// $ proto notation
// same limits as Proto() command
// advantage: easy to override properties
$hola(y=100 color=yellow)
```

### * Notation

Multiple copies can be made by using the * notation with the $ notation.

```script
// * count notation
// used to make multiple copies
$hello*3(y=-100 color=red)
```

### Overriding Properties

Overriding properties can be confusing because the scripting language uses enclosed properties in different ways, depending on the object or anim.  

For example, the Proto command has a count property and a script property which are used to specify the prototype definition and how many copies to make.  When using the $ notation, the prototype id is specified after the $ and the * nottion is used for the count.  That allows the enclosed properties to be applied to each prototype copy.

## @ Notation

The @ notation adds one or more anims to a parent object.

```script
// use the @ notation to add anims to a parent object
Text(x=-300 "testing" 
  anim(wait=1000 make="@(update('123'))")
)

// use the @ notation to add move anims to a parent object over time
// each move anim has a random y velocity and short duration
Oval(color=#ac2 x=300
  make("@(move(dy=~-100<100 dur=500))" count=22 destroy)
)
```

Note: see the [make anim](#the-make-anim) section for more details.

## The Basic Anim

The [anim](./anims.md#anim--listen) anim can be used to create a prototype copy when an anim is completed.  Since all anims inherit the properties of the basic anim, this can be quite powerful.

```script

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

Here are two examples of the [lerp](./anims.md#lerp--tween) anim and [move](./anims.md#move) anim inheriting the make property from the basic anim.

```script
// example: lerp anim creating a circle when done
// start hidden below the view
Text("Surprise!" y=500
  // wait 4 secs, fly into view, then make a flying circle 
  lerp(wait=4000 y=0 make="Circle(color=red move(dy=-100 destroy))")
)

// example: move anim creating multiple prototype copies when done
// start hidden below the view
Rect(y=500
  // move into view and then create 9 copies of the hello prototype
  move(wait=5000 dx=75 dy=-100 dur=5000 make="$hello*9(color=~red,yellow,orange)")
)
```

Since every anim has the make property, it is easy to chain logic by creating new objects when an anim is done.  The prototype definition can be simple or very complex.  

### Inline Quotes

Be careful with quotes to ensure the same quotes are not used to enclose the inline definition and some of the content within that definition.  Prototpye definitons enclosed by Define do not have that limitation.

## If Anim

The **if** anim can be used to create a prototype based on a decision.

```script

Def(
  randomValue=~0<100
)

Circle(x=-300
  if (q="randomValue > 75" true="@(lerp(y=100))" false="@(lerp(x=-400))")
)
```

The **if** anim can also be used to [change state](/state-machines.md#behavior-tree-construct) as part of a behavior tree construct.

## The Make Anim

The [make](./anims.md#make) anim provides a way to create prototype copies over time.

The prototyping methods described in the previous sections create prototype copies at a certain moment in time.  That is useful for chained logic and instantaneous particle effects.  When prototype copies need to be created over time, the make anim provides that functionality.  It can be used for things like particle trails and complex particle effects.

```script
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
    make(wait=5000 count=10 interval=500
        "Rect(x=-200 y=200
            rotate(inf)
            move(dx=100 dur=5000 des)
        )"
    )
)
```
