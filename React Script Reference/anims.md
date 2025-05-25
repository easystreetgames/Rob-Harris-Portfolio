# Anims

Anims perform specific actions, usually affecting the [display object](./objects.md) they are attached to.  All anims inherit the capabilites of the base anim in addition to their specific function.  Multiple anims can be attached to a display object to give it their combined capabilities.

## anim / listen
The base class for all anims. Provides core animation functionality and parameters common to all animation types.

### Parameters
- wait (int): delay in milliseconds before the animation starts
- block / b (boolean): if true, blocks subsequent animations until completed
- destroy / des (boolean): if true, destroys the object when animation completes
- in / inEvent (string): event that triggers this animation to begin
- out / outEvent (string): event to trigger when animation completes
- make / then / proto (string): ID of [prototype](./objects.md#prototypes) to create upon completion
- reset (boolean): if true, restarts the animation when it completes (see [tutorial](./reset.md))
- removeSelf / remove / rem (boolean): if true, removes the animation when it completes (default: true)

### Example
```
anim(wait=1000 out='start' block=true)
```

```
anim(in='gameStart' make='welcome')
```

## browse
Opens a URL in a new browser window or tab. Can optionally create a window with custom text content, or close a previously opened window.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / url (string): URL to open in the browser
- target (string): target window name (default: '_blank')
- features (string): window features configuration string
- text / txt / msg / content (string): custom text content to display in the window
- heading / head (string): heading text for the custom content window
- close (boolean): if true, closes a window with the specified target name instead of opening one

### Example
```
browse(url='https://example.com' target='helpWindow' features='width=800,height=600')
```

```
browse(target='helpWindow' close=true)
```

## destroy
Destroys a target object.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): the ID of the object to destroy

## if
Performs a conditional test and executes different actions based on the result. Can test variable values using various comparison operators.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): optional target object (default is self)
- testValue / test / q (string): the condition to test, can include operators (=, !=, <, >, <=, >=)
- trueValue / true / yes (string): action to perform if the test is true
- falseValue / false / no (string): action to perform if the test is false
- local (boolean): if true, tests a local variable on the target object rather than a global variable

### Example
```
if(test='score=100' true='win()' false='continue()')
```

```
if(test='health<20' true='damaged' false='normal')
```

```
if(target='player' test='lives<=0' true='gameOver()')
```

## lerp / tween
Linearly interpolates (transitions) object properties from their current values to specified target values over time.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- duration / dur (int): duration in milliseconds for the interpolation
- easing / e / ease (string): easing function to use ("in", "out", "in-out")
- targetX / x (int): target x-coordinate
- targetY / y (int): target y-coordinate
- targetW / w / width (int): target width
- targetH / h / height (int): target height
- targetAngle / angle (int): target rotation angle in degrees
- targetFrame / frame (int): target frame for sprite objects
- targetAlpha / alpha (float): target transparency value (0-1)
- targetScale / scale (float): target uniform scale value for both x and y
- count (int): number of times to repeat the animation
- reverse / rev / osc (boolean): if true, alternates between forward and reverse on each iteration
- infinity (boolean): if set, the animation repeats indefinitely

### Example
```
lerp(x=300 y=200 duration=1000 easing='out')
```

```
lerp(width=200 height=150 alpha=0.5 duration=500)
```

```
lerp(scale=2 angle=90 duration=2000 reverse=true count=3)
```

## load
Loads content from a text file and processes it as a script. Can optionally store the loaded content in a variable instead of processing it.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / file (string): path to the text file to load
- clear (boolean): if true, clears all objects before processing the loaded script
- key (string): if provided, stores the loaded content in a variable with this key instead of processing it
- manifest (boolean): if true, checks the manifest when loading the file

### Example
```
load(file='scripts/intro.txt' clear=true)
```

```
load(file='data/settings.txt' key='settings')
```
## make
Parses a prototype definition to create a new object or objects.  There are a variety of options during cloning.  The count parameter specifies how many times to repeat the cloning operation.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim properties)
- script (string): id of prototype to parse
- count (int): number of times to parse (default is 1)
- interval / int (int): delay in milliseconds before repeating
## Example
```
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

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim properties)
- vx (int): x velocity (pixels per second)
- vy (int): y velocity (pixels per second)
- duration / dur (int): duration in milliseconds

### Example
```
Rect(
  move(vx=100 vy=-100 dur=3000)
)
```

## rotate
Rotates an object based on rotation velocity.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim properties)
- velocity / vel (int): rotation velocity (degrees per second)
- duration / dur (int): duration in milliseconds

### Example
```
Rect(
  rotate(vel=90 duration=2000)
)
```

## set
Set one or more [variables](./objects.md#variables) when the anim becomes active There is also a [Define()](./objects.md#define) command used to set variables when the script is parsed (see [Object Reference document](./objects.md#define)).

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- key=value separated by spaces - associates a key with a value to be used as a variable

### Example
```
set(score=0 timer=60) 
```
## update
Sets one or more object properties. This anim can update any object property such as x, y, width, height, alpha, rotation, src, etc.

### Parameters
- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): optional target object (default is self)
- key=value separated by spaces - sets object properties

### Example
```
Text(text=’hello’ color=green
  update(wait=3000 text=’goodbye’ color=red)
)
```