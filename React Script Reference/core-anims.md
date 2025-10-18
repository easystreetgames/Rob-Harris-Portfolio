# Core Anims

Core anims perform fundamental actions, usually affecting the [display object](./objects.md) they are attached to. All anims inherit the capabilities of the base anim in addition to their specific function. Multiple anims can be attached to a display object to give it their combined capabilities.

## Basic Anim Parameters

**Every animation inherits these parameters from the base anim class.** Rather than repeating them for each anim below, they are documented once here:

- **wait** (int): Delay in milliseconds before the animation starts
- **block / b** (boolean): If true, blocks subsequent animations until completed
- **destroy / des** (boolean): If true, destroys the object when animation completes
- **in / inEvent** (string): Event that triggers this animation to begin
- **out / outEvent** (string): Event to trigger when animation completes
- **make / then / proto** (string): ID of [prototype](./prototypes.md) to create upon completion - see [prototypes tutorial](./prototypes.md)
- **reset** (boolean): If true, restarts the animation when it completes - see [reset tutorial](./reset.md)
- **removeSelf / remove / rem** (boolean): If true, removes the animation when it completes (default: true)
- **set** (key=value pairs): Set global variables upon completion

For detailed philosophy on how these properties enable powerful combinations, see [animation-philosophy.md](./animation-philosophy.md).

## anim / listen

The base class for all anims. Provides core animation functionality and parameters common to all animation types.

### Parameters for anim

All [basic anim parameters](#basic-anim-parameters) listed above.

### Example: anim

```script
anim(wait=1000 out='start' block=true)
```

```script
anim(in='gameStart' make='welcome')
```

## destroy

Destroys a target object.

### Parameters for destroy

- All [basic anim parameters](#basic-anim-parameters)
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

## lerp / tween

Linearly interpolates (transitions) object properties from their current values to specified target values over time. See [animation-recipes.md](./animation-recipes.md) for practical combination patterns.

### Parameters: lerp

- All [basic anim parameters](#basic-anim-parameters)
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

- All [basic anim parameters](#basic-anim-parameters)
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

Parses a prototype definition to create a new object or objects over time. See [prototypes.md](./prototypes.md#the-make-anim) for detailed usage.

### Parameters for make

- All [basic anim parameters](#basic-anim-parameters)
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

- All [basic anim parameters](#basic-anim-parameters)
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

- All [basic anim parameters](#basic-anim-parameters)
- velocity / vel / v (int): rotation velocity (degrees per second)
- duration / dur (int): duration in milliseconds

### Example: rotate

```script
Rect(
  rotate(vel=90 duration=2000)
)
```

## set

Set one or more variables when the anim becomes active. See [prefixes.md](./prefixes.md) for variable notation. There is also a [Define()](./objects.md#define) command used to set variables when the script is parsed.

### Parameters for set

- All [basic anim parameters](#basic-anim-parameters)
- key=value separated by spaces - associates a key with a value to be used as a variable

### Example: set

```script
set(score=0 timer=60)
```

## anchor

Anchors objects to specific positions on the canvas edges or corners. Executes instantly to position the object relative to the canvas boundaries with optional offset adjustments.

### Parameters for anchor

- All [basic anim parameters](#basic-anim-parameters)
- anchor / to / position / pos / text (string): anchor position - 'left', 'right', 'top', 'bottom', 'center', 'topleft', 'topright', 'bottomleft', 'bottomright'
- offsetX / ox / dx (int): horizontal offset from the anchor position in pixels (default: 0)
- offsetY / oy / dy (int): vertical offset from the anchor position in pixels (default: 0)

### Example: anchor

```script
// Anchor to left edge with offset
Text(text='LEFT' anchor(to='left' ox=10 oy=20))
```

```script
// Anchor to bottom-right corner
Text(text='BOTTOM RIGHT' anchor(to='bottomright' ox=10))
```

```script
// Center positioning with offset
Rect(
  anchor(to='center' offsetX=50 offsetY=-30)
)
```

## atlas

Loads a CSV atlas file and cuts an ImageObject's source into separate frame images that are stored in the image pool for later use. The CSV file should have columns for name, x, y, width, and height to define sprite frames.

### Parameters for atlas

- All [basic anim parameters](#basic-anim-parameters)
- csv / atlas / key / text (string): key name of the CSV data to use for cutting frames

### Example: atlas

```script
// Load CSV atlas data first, then cut frames from image
Image(src='spritesheet.png'
  load('csv/sprites.csv' block)
  atlas('sprites' make='createSprites')
)
```

```script
// Complete example with frame usage
Def(
  Image(id=mainAtlas src='atlas.png'
    load('csv/gameAtlas.csv' block)
    atlas('gameAtlas' make='showFrames')
  )

  _(id=showFrames
    Image(x=-200 src='player_idle')  // Uses frame from atlas
    Image(x=200 src='coin')          // Uses frame from atlas
  )
)

$mainAtlas()
```

## hover

Handles mouse hover interactions on sprite objects with visual feedback through frame changes. Can specify different frames for hover states, mouse down, and normal states. See [animation-recipes.md](./animation-recipes.md#ui--interaction-recipes) for UI interaction patterns.

### Parameters for hover

- All [basic anim parameters](#basic-anim-parameters)
- target / t / text (string): ID of target object to apply hover effects to (default: self)
- trueFrame / true (int): frame to display when hovering (default: 0)
- falseFrame / false (int): frame to display when not hovering (default: 0)
- downFrame / down (int): frame to display when mouse is down while hovering (default: same as trueFrame)
- once (boolean): if true, only trigger once then complete animation

### Example: hover

```script
// Basic hover effect changing sprite frames
Sprite(src='button.png' frame=0
  hover(false=0 true=1 down=2)
)
```

```script
// Hover with one-time trigger creating objects
Sprite(src='dice.png' frame=0 scale=0.5
  hover(false=0 true=1 once make='@$flyingDice()')
)
```

```script
// Hover affecting a different target object
Sprite(id=button src='button.png'
  hover(target='otherSprite' false=0 true=1 down=2)
)
```

## lerppath

Animates objects along predefined paths using linear or bezier curve interpolation. Extends the lerp animation with path-specific movement functionality, supporting all lerp parameters like count, reverse, and easing.

### Parameters for lerppath

- All [basic anim parameters](#basic-anim-parameters)
- duration / dur (int): duration in milliseconds for path traversal
- easing / e / ease (string): easing function ("in", "out", "in-out")
- count (int): number of times to repeat the path animation
- reverse / rev / osc (boolean): if true, alternates direction on each repetition
- infinity (boolean): if set, repeats the path animation indefinitely
- path / points / route (string): path points in format "x1,y1 x2,y2 x3,y3..."
- bezier / curve / smooth (boolean): if true, uses smooth bezier curves between points
- csv (string): key name of CSV data containing path points
- connectStart / connect (boolean): if true, connects the last point back to the first

### Example: lerppath

```script
// Basic linear path
Circle(
  path(points='300,100 300,200 -200,200' dur=6000)
)
```

```script
// Smooth bezier curve path with oscillation
Circle(
  path(points='300,100 300,200 -200,200' bezier=true dur=3000 osc inf ease=in)
)
```

```script
// Complex multi-point bezier path
Circle(
  path(points='400,100 -200,50 -300,150 300,-200 400,150'
       bezier=true dur=4000 osc inf ease=in-out)
)
```

```script
// Path from CSV data with connection to start
Circle(
  load('csv/pathData.csv' block)
  path(csv='pathData' dur=9000 ease=in-out inf connect)
)
```

## fetch

Fetches values from global or local variables and sets object properties. Useful for transferring data from variables to object properties for dynamic updates. See [prefixes.md](./prefixes.md) for variable notation.

### Parameters for fetch

- All [basic anim parameters](#basic-anim-parameters)
- target / t / text (string): ID of target object to modify (default: self)
- key / keyIn (string): name of the variable to fetch the value from
- property / prop (string): name of the object property to set
- local (boolean): if true, fetches from local variables on the object instead of global

### Example: fetch

```script
// Fetch global variable to set object property
Define(playerScore=1500)
Text(text='Score: 0'
  fetch(key='playerScore' property='text')
)
```

```script
// Fetch to a different target object
Define(currentLevel=3)
Text(id='levelDisplay' text='Level: 1')
Empty(
  fetch(target='levelDisplay' key='currentLevel' property='text')
)
```

```script
// Fetch from local variable
Circle(
  local(radius=50 color='red')
  fetch(key='color' property='color' local)
)
```

## local

Sets local variables on objects instead of global variables. Local variables are scoped to the specific object and can be accessed by that object's animations using the local parameter. See [prefixes.md](./prefixes.md) for local variable notation.

### Parameters for local

- All [basic anim parameters](#basic-anim-parameters)
- key=value pairs: any number of key-value assignments for local variables

### Example: local

```script
// Set local variables on an object
Circle(
  local(health=100 maxHealth=100 level=1)
  update(text='Health: $health' local)
)
```

```script
// Local variables for object-specific state
Text(text='Player'
  local(x=100 y=200 speed=5)
  move(dx=$speed dur=1000 local reset)
)
```

```script
// Multiple objects with different local variables
Def(
  Text(id=player1
    local(score=0 lives=3)
    update(text='Player 1: $score' local)
  )
  Text(id=player2
    local(score=0 lives=3)
    update(text='Player 2: $score' local)
  )
)

$player1(y=100)
$player2(y=200)
```

## update

Sets one or more object properties instantly. This anim can update any object property such as x, y, width, height, alpha, rotation, src, etc. See [reset.md](./reset.md) for continuous update patterns.

### Parameters for update

- All [basic anim parameters](#basic-anim-parameters)
- target (string): optional target object (default is self)
- key=value separated by spaces - sets object properties

### Example: update

```script
Text(text='hello' color=green
  update(wait=3000 text='goodbye' color=red)
)
```