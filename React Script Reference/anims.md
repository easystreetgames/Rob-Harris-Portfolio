# Anims

Anims perform specific actions, usually affecting the [display object](./objects.md) they are attached to.  All anims inherit the capabilites of the base anim in addition to their specific function.  Multiple anims can be attached to a display object to give it their combined capabilities.

## anim / listen

The base class for all anims. Provides core animation functionality and parameters common to all animation types.

### Parameters for anim

- wait (int): delay in milliseconds before the animation starts
- block / b (boolean): if true, blocks subsequent animations until completed
- destroy / des (boolean): if true, destroys the object when animation completes
- in / inEvent (string): event that triggers this animation to begin
- out / outEvent (string): event to trigger when animation completes
- make / then / proto (string): ID of [prototype](./objects.md#prototypes) to create upon completion
- reset (boolean): if true, restarts the animation when it completes (see [tutorial](./reset.md))
- removeSelf / remove / rem (boolean): if true, removes the animation when it completes (default: true)

### Example: anim

```script
anim(wait=1000 out='start' block=true)
```

```script
anim(in='gameStart' make='welcome')
```

## browse

Opens a URL in a new browser window or tab. Can optionally create a window with custom text content, or close a previously opened window.

### Parameters for browse

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / url (string): URL to open in the browser
- target (string): target window name (default: '_blank')
- features (string): window features configuration string
- text / txt / msg / content (string): custom text content to display in the window
- heading / head (string): heading text for the custom content window
- close (boolean): if true, closes a window with the specified target name instead of opening one

### Example: browse

```script
browse(url='https://example.com' target='helpWindow' features='width=800,height=600')
```

```script
browse(target='helpWindow' close=true)
```

## destroy

Destroys a target object.

### Parameters for destroy

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): the ID of the object to destroy

### Example: destroy

```script
Rect(
  destroy(wait=3000)
)
```

```script
Oval(id=testObj)

Empty(
  destroy(target=testObj wait=3000)
)
```

## if / compare

Performs a conditional test and executes different actions based on the result. It can test variable values using various comparison operators.

### Parameters for if

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): optional target object (default is self)
- test / q (string): the condition to test, can include operators (=, !=, <, >, <=, >=)
- true / yes (string): state to set or prototype to make if the test is true
- false / no (string): state to set or prototype to make if the test is false
- local (boolean): if true, tests a local variable on the target object rather than a global variable

#### Blocking

The **if** anim can be used as a blocking anim until a condition is met.

#### Change State

An object's state can be changed when a condition is met (See the [State Machine Tutorial](./state-machines.md) for more details).  

#### Prototypes

A prototype definition can be used to make a copy of the prototype when a condition is met. (See the [Prototypes](/prototypes.md#if-anim) for more details).

### Example: if

```script
if(test='score=100' true='$win()' false='$continue()')
```

```script
if(test='health<20' true='damaged' false='normal')
```

```script
if(target='player' test='lives<=0' true='$gameOver()')
```

## lerp / tween

Linearly interpolates (transitions) object properties from their current values to specified target values over time.

### Parameters: lerp

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- duration / dur (int): duration in milliseconds for the interpolation
- easing / e / ease (string): easing function to use ("in", "out", "in-out")
- x (int): target x-coordinate
- y (int): target y-coordinate
- w / width (int): target width
- h / height (int): target height
- angle (int): target rotation angle in degrees
- frame (int): target frame for sprite objects
- alpha (float): target transparency value (0-1)
- scale (float): target uniform scale value for both x and y
- count (int): number of times to repeat the animation
- target (string): optional target object (default is self)
- reverse / rev / osc (boolean): if true, alternates between forward and reverse on each iteration
- infinity (boolean): if set, the animation repeats indefinitely

### Example: lerp

```script
lerp(x=300 y=200 duration=1000 easing='out')
```

```script
lerp(width=200 height=150 alpha=0.5 duration=500)
```

```script
lerp(scale=2 angle=90 duration=2000 reverse=true count=3)
```

## load

Loads content from a text file and processes it as a script. Can optionally store the loaded content in a variable instead of processing it.

### Parameters for load

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / file (string): path to the text file to load
- clear (boolean): if true, clears all objects before processing the loaded script
- key (string): if provided, stores the loaded content in a variable with this key instead of processing it
- manifest (boolean): if true, checks the manifest when loading the file

### Example load

```script
load(file='scripts/intro.txt' clear=true)
```

```script
load(file='data/settings.txt' key='settings')
```

## make

Parses a prototype definition to create a new object or objects.  There are a variety of options during cloning.  The count parameter specifies how many times to repeat the cloning operation.

### Parameters for make

- wait, in, out, block, destroy, reset, make (basic anim properties)
- script (string): id of prototype to parse
- count (int): number of times to parse (default is 1)
- interval / int (int): delay in milliseconds before repeating

## Example: make

```script
Define(
  Rect(id=cube1 
    move(vx=100 dur=3000 destroy=true)
  )
)

// create a total of 10 cubes over time
Cont(
  make(script=cube1 count=10 interval=1000)
)
```

## move

Moves an object based on x and y velocity.

### Parameters for move

- wait, in, out, block, destroy, reset, make (basic anim properties)
- vx (int): x velocity (pixels per second)
- vy (int): y velocity (pixels per second)
- velocity / vel / v (int): direction velocity (based on current rotation)
- duration / dur (int): duration in milliseconds

### Example: move

```script
Rect(
  move(vx=100 vy=-100 dur=3000)
)
```

```script
Rect(
  move(v=100 inf)
  rotate(inf)
)
```

## rotate

Rotates an object based on rotation velocity.

### Parameters for rotate

- wait, in, out, block, destroy, reset, make (basic anim properties)
- velocity / vel / v (int): rotation velocity (degrees per second)
- duration / dur (int): duration in milliseconds

### Example: rotate

```script
Rect(
  rotate(vel=90 duration=2000)
)
```

## set

Set one or more [variables](./objects.md#variables) when the anim becomes active There is also a [Define()](./objects.md#define) command used to set variables when the script is parsed (see [Object Reference document](./objects.md#define)).

### Parameters for set

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- key=value separated by spaces - associates a key with a value to be used as a variable

### Example: set

```script
set(score=0 timer=60) 
```

## update

Sets one or more object properties. This anim can update any object property such as x, y, width, height, alpha, rotation, src, etc.

### Parameters for update

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): optional target object (default is self)
- key=value separated by spaces - sets object properties

### Example: update

```script
Text(text=’hello’ color=green
  update(wait=3000 text=’goodbye’ color=red)
)
```
