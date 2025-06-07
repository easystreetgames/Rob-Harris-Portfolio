# Reset Property

The **reset** property resets an anim's state after it has completed.  This causes various effects, depending on the anim. Every anim inherits the **reset** property from the basic anim.

In most of the examples, the **wait** property is used to add a delay between the anim repetitions.

## Basic Anim

The **anim** anim can repeat any of its functions, such as firing events, listening for events, and making prototype copies. The **move** anim inherits the basic anim's properties, so it can repeatedly listen and respond to an event as in this example.

```script
// --- basic anim and move anim ---

// moving squares
_(
  // fire a tick event once per second
  anim(wait=2000 out=tick reset)

  Square(y=-200 color=green
    // wait for tick event, move a little, then listen for another tick event
    move(in=tick dx=50 reset)
  )

  Square(y=-150 color=red
    // same logic moving in the opposite direction
    move(in=tick dx=-50 reset)
  )
)

// flying ovals
_(
  // make a flying oval every 3 seconds
  anim(wait=3000 reset
    make="Oval(move(dy=100 destroy))"
  )
)
```

## Move and Rotate Anims

The **move** and **rotate** anims can repeat thier motion after a delay.

```script
// --- move anim ---

Square(x=-300
  move(wait=1000 vx=50 dur=2000 reset)
)

// --- rotate anim ---

Square(
  rotate(wait=1000 dur=3000 reset)
)
```

## Lerp Anim

The **lerp** anim can perform a more complex timing by repeating it cycle as specified, then pausing before repeating the cycle again.

```script
// --- lerp anim ---

Oval(color=green
  // perform a hopping motion
  lerp(wait=2000 y=-150 h=40 dur=500 count=2 ease=out osc reset)
)
```

## Destroy Anim

The **destroy** anim can sequentially destroy objects with the same ID from back z-order to front.

```script
// --- destroy anim ---

_(
  // make 5 test objects
  make(count=5 "Rect(id=testRect x=~-200<200 y=~-200<200)" b)
  // destroy them in creation order
  destroy(wait=2000 "testRect" reset)
)
```

## Load Anim

The **load** anim can load the same script file repeatedly.

```script
// --- load anim ---

_(
  load(wait=3000 'sample-bounce.txt' reset) 
)
```

## Add and Update Anims

The **add** anim can continuously add a value or string to a global variable.  The **update** anim resets after updating so that it will work again the next time the variable is changed.  This can be used for a countdown or timer.

```script
// --- add and update anims ---

Text(text='0' y=-150
  // increment a counter
  add(wait=1000 key=count1 v=1 block reset) 
  // update the display
  update(text=$count1 reset)
)

Text(text='' y=-100
  add(wait=5000 key=string1 v=V block reset) 
  update(text=$string1 reset)
)
```

## If Anim

The [if anim](/anims.md#if--compare) can check for a condition, change a state, then continue to check the condition.

```script
// --- if anim ---

Rect(id=testobj local='status=working' w=200
    state(id=cw rotate(vel=-45 inf))
    state(id=ccw rotate(vel=180 inf))
)

_(id=tester
  // continuously test the status
  if('status=ready' target=testobj true=1 false=0 reset local)

  // change the status over time
  set(wait=3000 local.testobj.status=ready b)
  set(wait=3000 local.testobj.status=busy b)
  set(wait=3000 local.testobj.status=ready b)
)
```
