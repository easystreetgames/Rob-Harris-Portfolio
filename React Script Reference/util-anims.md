# Utility Anims

Utility anims provide conditional logic, comparison operations, and other utility functions for script control flow and data manipulation.

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

A prototype definition can be used to make a copy of the prototype when a condition is met. (See the [Prototypes](./prototypes.md#if-anim) for more details).

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

## compare

Compares two variables using various operators and executes different actions based on the result. Unlike the `if` anim which uses a single test string, `compare` separates the comparison operands into distinct parameters.

### Parameters for compare

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target (string): optional target object (default is self)
- key / key1 (string): first variable to compare
- key2 / match (string): second variable to compare
- operator (string): comparison operator (=, ==, !=, <, >, <=, >=) - default is '='
- type (string): comparison type ('str' for string, 'num' for numeric) - default is 'str'
- numeric / num / number (boolean): shortcut to set type to 'num'
- true / yes (string): state to set or prototype to make if the comparison is true
- false / no (string): state to set or prototype to make if the comparison is false
- local (boolean): if true, compares local variables on the target object rather than global variables
- repeat / rep / inf / infinity / infinite (boolean): if true, keeps repeating the comparison

### Example: compare

```script
compare(key='score' key2='highScore' operator='>' true='newRecord' false='normalScore')
```

```script
compare(key='health' match='maxHealth' operator='=' true='fullHealth' numeric)
```

```script
compare(target='enemy' key='state' key2='aggressive' true='$attack()' local)
```

## arrange

Arranges child objects within a container (EmptyObject) in either horizontal or vertical layouts. The animation executes instantly to position all objects based on the specified direction and gap spacing.

### Parameters for arrange

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- direction / dir / orientation / layout (string): layout direction - 'horizontal', 'vertical', 'h', or 'v' (default: 'horizontal')
- gap / spacing / distance / padding (number): space between objects in pixels (default: 10)

### Example: arrange

```script
// Horizontal arrangement with default settings
_(
  arrange()
  Text(text='Item 1')
  Text(text='Item 2')
  Text(text='Item 3')
)
```

```script
// Vertical arrangement with custom gap
_(
  arrange(direction=vertical gap=20)
  Circle(radius=30 color=red)
  Circle(radius=40 color=orange)
  Circle(radius=35 color=green)
)
```

```script
// Using shorthand direction parameter
_(
  arrange(dir=v spacing=15)
  Text(text='Top')
  Text(text='Middle')
  Text(text='Bottom')
)
```

## add / inc

Adds values to global or local variables. Can perform numeric addition for counters, string concatenation for building text, or add rows to CSV/TSV data. Supports time formatting and prefix handling.

### Parameters for add

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- key / text (string): name of the variable to modify
- value / v (string/number): value to add (numeric for addition, string for concatenation)
- string / str (string): force string concatenation mode
- prefix / pre (string): prefix to add to the result
- time / t (boolean): if true, treats values as time (minutes/seconds conversion)
- csvKey / csv (string): key of CSV data to add a row to
- tsvKey / tsv (string): key of TSV data to add a row to
- local (boolean): if true, modifies local variable on the object instead of global

### Example: add

```script
// Basic numeric counter
Define(count=0)
Text(text='0'
  add(wait=1000 key='count' v=1 block reset)
  update(text=$count reset)
)
```

```script
// String concatenation
Define(message='Hello')
Text(
  add(key='message' string=' World!')
  update(text=$message)
)
```

```script
// Time-based counter with prefix
Define(timer=0)
Text(
  add(key='timer' v=1 time prefix='Time: ' block reset)
  update(text=$timer reset)
)
```

```script
// Add to CSV data
Text(
  add(csvKey='gameData' string='player,100,level1')
)
```

## mult

Multiplies global or local variables by a numeric value. Useful for scaling counters, applying multipliers, or performing mathematical operations on stored values.

### Parameters for mult

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- key (string): name of the variable to modify
- value / v (number): value to multiply by

### Example: mult

```script
// Double a score value
Define(score=50)
Text(
  mult(key='score' v=2)
  update(text=$score)
)
```

```script
// Apply damage multiplier
mult(key='damage' value=1.5)
```

## audio

Plays audio files with control over looping, muting, and destruction. Can play sounds once, loop continuously, or play for a specific duration before stopping.

### Parameters for audio

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / file / src / text (string): path to the audio file to play
- loop (boolean): if true, loops the audio indefinitely
- mute / muteKey (boolean): if true, mutes the audio

### Example: audio

```script
// Play audio once
Text(text='Play Sound'
  audio(in=click.button path='audio/click.mp3')
)
```

```script
// Play audio with auto-destroy after 3 seconds
Text(text='Play Fragment'
  audio(in=click.button path='audio/music.mp3')
  destroy(in=click.button wait=3000)
)
```

```script
// Loop background music
Text(text='Start Music'
  audio(in=click.button path='audio/background.mp3' loop)
)
```

```script
// Play with immediate destruction (short sound)
Text(text='Quick Sound'
  audio(in=click.button path='audio/beep.mp3' destroy)
)
```

## dovetail

Aligns objects so their edges almost touch, creating precise positioning relationships. Useful for creating connected layouts, tooltips, or UI elements that need to be positioned relative to other objects.

### Parameters for dovetail

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target / t (string): ID of the target object to align against
- anchor / to / position / pos / text (string): anchor position relative to target - 'top', 'bottom', 'left', 'right'
- gap / spacing / distance / offset (int): space between objects in pixels (default: 0)
- infinity / inf (boolean): if true, continuously updates alignment

### Example: dovetail

```script
// Position text below a title with gap
_(
  Text(id='title' text='Main Title')
  Text(id='subtitle' text='Subtitle'
    dovetail(target='title' anchor='bottom' gap=10)
  )
)
```

```script
// Continuous alignment during animations
_(
  Circle(id='anchor' radius=5
    dovetail(target='movingImage' anchor='top' gap=0 inf)
  )
  Image(id='movingImage' src='test.png'
    lerp(scale=1.5 dur=2000 osc inf)
  )
)
```

```script
// Complex layout with multiple dovetails
_(
  Text(id='header' text='Header')
  Text(id='content' text='Content...'
    dovetail(target='header' anchor='bottom' gap=20)
    dovetail(target='footer' anchor='top' gap=10)
  )
  Text(id='footer' text='Footer')
)
```

## call

Calls registered TypeScript functions from scripts. Functions must be pre-registered using `CallAnim.registerFunction()` before they can be called. Useful for bridging script execution with custom code functionality.

### Parameters for call

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- function / func / fn / text (string): name of the registered function to call
- parameters / params / args (string): comma-separated parameters to pass to the function

### Example: call

```script
// Basic function call with no parameters
Text(text='Execute Function'
  call(function='myCustomFunction')
)
```

```script
// Function call with parameters
Text(text='Call with Args'
  call(func='processData' params='arg1,arg2,arg3')
)
```

```script
// Function call triggered by event
Text(text='Click Me'
  call(in=click.button fn='handleClick' args='buttonId,123')
)
```

## filter / copy

Filters and processes text strings in variables using various filter types. Can extract JSON, format text, resolve variables, or retrieve stored data from cookies/session storage.

### Parameters for filter

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- keyIn / key (string): name of the variable containing the text to filter
- filter / type (string): type of filter to apply - 'json', 'paragraph', 'memory', 'variables', 'cookie', 'session', 'time'
- keyOut (string): name of the variable to store the filtered result (default: 'json')
- local (boolean): if true, uses local variables instead of global

### Example: filter

```script
// Extract JSON from LLM response
Text(
  llm(p='Create a JSON task list' block)
  filter(keyIn='response' filter='json' keyOut='taskData')
  update(text=$taskData)
)
```

```script
// Format text with paragraph spacing
Define(rawText='Line1\nLine2\nLine3')
Text(
  filter(key='rawText' type='paragraph' keyOut='formatted')
  update(text=$formatted)
)
```

```script
// Resolve variables in text
Define(template='Hello $userName, your score is $score')
Text(
  filter(key='template' type='variables' keyOut='message')
  update(text=$message)
)
```

```script
// Retrieve data from browser storage
Text(
  filter(keyIn='userData' type='cookie' keyOut='savedData')
  update(text=$savedData)
)
```

## log

Outputs log messages, warnings, and errors to the browser console. Supports different log levels and can track time deltas between operations for debugging purposes.

### Parameters for log

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- error / err (boolean): if true, logs as error level
- warning / warn (boolean): if true, logs as warning level
- log / t / text (string): message text to log
- delta / showDelta (boolean): if true, shows time delta since last log
- clearDelta / clear (boolean): if true, resets the delta timer

### Example: log

```script
// Basic console logging
Text(
  log(text='Script execution started')
)
```

```script
// Error logging
Text(
  log(error text='Critical error occurred')
)
```

```script
// Warning with delta timing
Text(
  log(warn text='Performance warning' delta)
)
```

## preload

Preloads assets and resources for later use. Helps improve performance by loading resources in advance before they are needed.

### Parameters for preload

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: preload

```script
// Basic preloading
Text(
  preload()
)
```

## report

Generates status reports and system information. Useful for debugging and monitoring the state of the animation system.

### Parameters for report

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: report

```script
// Generate system report
Text(
  report()
)
```