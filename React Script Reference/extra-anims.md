# Extra Anims

Extra anims provide advanced functionality for browser interaction, file operations, and specialized behaviors.

## browse

Opens a URL in a new browser window or tab. Can optionally create a window with custom text content, clear content, or close a previously opened window.

### Parameters for browse

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- path / url (string): URL to open in the browser
- target / t (string): target window name (default: '_blank')
- features (string): window features configuration string
- text / txt / msg / content (string): custom text content to display in the window
- heading / head (string): heading text for the custom content window
- close (boolean): if true, closes a window with the specified target name instead of opening one
- clear / clr (boolean): if true, clears existing content before adding new content

### Example: browse

```script
// Open a URL in a new window
browse(url='https://google.com')
```

```script
// Open a custom window with specific features
browse(url='https://example.com' target='helpWindow' features='width=800,height=600')
```

```script
// Display custom content in a new window
browse(content='Hello from the animation system!' heading='Custom Content' features='width=400,height=300')
```

```script
// Display HTML content from a variable
Define(html='<h1>Hello</h1> from <b>the</b> vars')
browse(content=$html features='width=400,height=400,menubar=no,toolbar=no,location=no')
```

```script
// Close a previously opened window
browse(target='helpWindow' close=true)
```

## separate

Separates text content into individual characters or words, creating multiple objects from a prototype for each separated element. Useful for creating text animation effects where individual parts need different behaviors.

### Parameters for separate

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- proto / prototype (string): ID of the prototype to create for each separated element
- mode (string): separation mode - 'chars' for individual characters, 'word' for words
- interval / int (int): delay in milliseconds between creating each element
- destroy / des (boolean): if true, destroys the original text object after separation

### Example: separate

```script
// Separate text into individual characters with animation
Def(
  Text(id=charProto text=$p_char
    move(wait=$p_wait dy=-100 dur=3000 destroy)
  )
)

Text(text='Hello World' y=100
  separate(wait=1000 proto='charProto' mode='chars' interval=200 destroy)
)
```

```script
// Separate into words with different effects
Def(
  Text(id=wordProto text=$p_word
    lerp(rotation=360 dur=2000 block)
    move(thrust=150 dur=4000 destroy)
  )
)

Text(text='Welcome to ESG Scripting'
  separate(proto='wordProto' mode='word' interval=500)
)
```

```script
// Character separation with staggered delays
Def(
  Text(id=flyingChar text=$p_char size=40
    lerp(y=-200 scale=2 dur=1000 ease=out block)
    fade(alpha=0 dur=500 destroy)
  )
)

Text(text='AMAZING!' size=60 y=0
  separate(proto='flyingChar' mode='chars' interval=300 destroy)
)
```

## assert

Assertion testing for object properties. Verifies that object properties match expected values and can trigger events or reports based on the test results.

### Parameters for assert

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- property=value pairs: properties to test with their expected values
- report (boolean): if true, logs the assertion result to console

### Example: assert

```script
// Test object position after movement
Rect(id='testRect'
  lerp(x=200 block)
  assert(x=200 report out='positionVerified')
)
```

```script
// Chain assertions with event triggers
Rect(
  lerp(y=200 block)
  assert(y=200 report out='step1')
  lerp(in='step1.success' x=100 block)
  assert(x=100 report)
)
```

## download

Downloads data as files to the user's computer. Can download text content, variables, or CSV/TSV data with automatic filename generation.

### Parameters for download

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- text (string): direct text content to download
- key (string): variable name containing content to download
- csv (string): CSV data key to download as CSV file
- tsv (string): TSV data key to download as TSV file

### Example: download

```script
// Download direct text content
Text(text='Download Text'
  download(text='Hello World\nThis is a test file')
)
```

```script
// Download variable content
Define(report='Game Statistics:\nScore: 1500\nLevel: 3')
Text(text='Download Report'
  download(key='report')
)
```

```script
// Download CSV data
Text(
  load('gameData.csv' block)
  add(csv='gameData' string='newPlayer,100,level1')
  download(csv='gameData')
)
```

## fence

Creates boundary detection and collision handling for moving objects. Objects can bounce, wrap, or be destroyed when they hit fence boundaries.

### Parameters for fence

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- target / text (string): ID of the fence boundary object (usually a Rect)
- inside (boolean): if true, keeps object inside the fence; if false, keeps object outside
- bounce (boolean): if true, reverses direction when hitting boundary
- wrap (boolean): if true, teleports to opposite side when hitting boundary
- block (boolean): if true, blocks further movement until object leaves boundary

### Example: fence

```script
// Bouncing ball inside rectangle
Circle(
  move(dx=100 dy=50 inf)
  fence(target='boundary' inside bounce)
)
Rect(id='boundary' w=300 h=200 stroke='black')
```

```script
// Wrapping movement at boundaries
Circle(
  move(dx=75 inf)
  fence(target='playArea' inside wrap)
)
```

```script
// Destroy object when it leaves boundary
Circle(
  move(dx=100 inf)
  fence(target='safeZone' inside block)
  lerp(alpha=0 dur=500 destroy)
)
```

## format

Formats text or code strings for display or processing. Takes text content and applies formatting rules to make it more readable or structured.

### Parameters for format

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- key (string): variable name containing the text to format
- result (string): variable name to store the formatted result

### Example: format

```script
// Format code for display
Define(
  rawCode='_(Text(id=proto move(dx=100) lerp(alpha=0)))'

  Text(id=display text=$formatted align=left)
)

Text(
  format(key='rawCode' result='formatted' make='display')
)
```

## pixel

Scans images for pixels matching specific colors and extracts coordinate data. Useful for creating maps, collision detection data, or path finding information from images.

### Parameters for pixel

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- color (string): hex color code to scan for (e.g., 'ff0000' for red)
- csv (string): CSV data key to store the pixel coordinates
- threshold (int): color matching tolerance (0-255, default: 0)

### Example: pixel

```script
// Scan image for red pixels and save coordinates
Image(src='map.png'
  load('csv/pixelData.csv' block)
  pixel(color='ff0000' csv='pixelData' threshold=50)
  download(csv='pixelData')
)
```

```script
// Create path data from image
Image(src='pathMap.png'
  pixel(color='00ff00' csv='pathPoints' threshold=10)
  make('createPath')
)
```

## pixelate

Creates pixelation effects by converting images into individual pixel objects. Each pixel becomes a separate object that can be animated independently.

### Parameters for pixelate

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- size (int): size of each pixel block (default: 1)
- proto / prototype (string): ID of prototype to create for each pixel
- interval / int (int): delay between creating each pixel object
- destroy / des (boolean): if true, destroys the original image after pixelation

### Example: pixelate

```script
// Basic pixelation effect
Def(
  Rect(id=pixelProto w=5 h=5
    move(dx=random(-100,100) dy=random(-100,100) dur=2000)
    fade(alpha=0 dur=1000 destroy)
  )
)

Image(src='photo.png'
  pixelate(size=5 proto='pixelProto' interval=10 destroy)
)
```

```script
// Animated pixelation with delayed creation
Def(
  Circle(id=dotPixel radius=2
    move(wait=$p_wait dx=random(-300,300) dy=random(-300,300) inf)
    lerp(wait=random(1000,3000) alpha=0 dur=500 destroy)
  )
)

Image(src='texture.png'
  pixelate(in='startEffect' size=8 proto='dotPixel' interval=5)
)
```

## save

Saves data or state information to persistent storage. Can save variables, object states, or game progress for later retrieval.

### Parameters for save

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: save

```script
// Basic save operation
Text(text='Save Game'
  save()
)
```

## cookie

Stores and retrieves values using browser localStorage or sessionStorage. Provides persistent data storage that survives browser sessions (localStorage) or temporary storage for the current session (sessionStorage).

### Parameters for cookie

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- session (boolean): if true, uses sessionStorage instead of localStorage
- key=value pairs: data to store in browser storage

### Example: cookie

```script
// Store data in localStorage
Text(
  cookie(userName='John' score=1500 level=3)
)
```

```script
// Store in sessionStorage (temporary)
Text(
  cookie(session gameState='paused' currentLevel=5)
)
```

```script
// Retrieve stored data using filter
Text(
  filter(keyIn='userName' type='cookie' keyOut='savedName')
  update(text='Welcome back $savedName')
)
```

## deck

Card deck operations for creating, shuffling, and managing playing cards. Useful for implementing card games with standard deck management functionality.

### Parameters for deck

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- numCards / cards (int): number of cards in the deck
- startingIndex / start (int): starting index for card numbering
- cardBack / back (string): image source for card backs

### Example: deck

```script
// Create standard 52-card deck
Text(
  deck(cards=52 start=1 back='cardBack.png')
)
```

## diff

Compares two strings and finds differences between them. Useful for text comparison, change detection, or validation purposes.

### Parameters for diff

- wait, in, out, block, destroy, reset, make (basic anim parameters)
- keyIn / key (string): first string variable to compare
- keyIn2 / key2 (string): second string variable to compare
- keyOut (string): variable to store the difference result

### Example: diff

```script
// Compare two text values
Define(
  originalText='Hello World'
  modifiedText='Hello Universe'
)

Text(
  diff(key='originalText' key2='modifiedText' keyOut='changes')
  update(text=$changes)
)
```

## lint

Code validation and linting functionality. Checks code syntax and style according to specified rules.

### Parameters for lint

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: lint

```script
// Basic code validation
Text(
  lint()
)
```

## scroll

Scrolling controls for managing viewport and camera movement. Provides smooth scrolling animations and scroll position management.

### Parameters for scroll

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: scroll

```script
// Basic scrolling
Text(
  scroll()
)
```

## top

Moves objects to the top layer in the z-order, ensuring they appear above other objects. Useful for bringing UI elements or important objects to the front.

### Parameters for top

- wait, in, out, block, destroy, reset, make (basic anim parameters)

### Example: top

```script
// Bring object to front
Text(text='Important Message'
  top()
)
```