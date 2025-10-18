# Prefixes

ESG Script uses various prefixes to enable different types of value resolution and object references. These prefixes are processed by the PropertyTransformerRegistry and ObjectFactory.

## Property Value Prefixes

Property values can use special prefixes to enable dynamic value resolution:

### `~` Random Notation

Generates random values at runtime.

**Syntax:**
- `~min<max` - Random number in range (e.g., `~1<10`)
- `~option1,option2,option3` - Random item from list

**Examples:**
```
Circle(x=~100<500 y=~50<300 color=~red,blue,green)
Text(text=~Hello,Hi,Greetings x=~0<800)
```

### `$` Global Variable References

References a value from the global variables store.

**Syntax:** `$variableName`

**Examples:**
```
// Set a global variable
set(playerName='Alice')

// Reference it in properties
Text(text=$playerName x=100 y=100)
Circle(x=$targetX y=$targetY)
```

### `<<` Embedded Global Variable References

Embeds global variable values within text strings. Multiple variables can be embedded in a single string.

**Syntax:** `<<variableName>>`

**Examples:**
```
set(playerName='Alice' score=100)
Text(text='Player: <<playerName>> Score: <<score>>')
```

### `%` Object (Local) Variable References

References a local variable stored on a game object.

**Syntax:**
- `%variableName` - References local variable from current object
- `%self%` - Special reference that resolves to the object's own ID

**Examples:**
```
// Set local variable on object
_(id=player @(local(health=100 score=0)))

// Reference local variable
Text(text=%health target=player)
Circle(radius=%size)
```

### `^` CSV Variable References

Fetches values from loaded CSV data stored in the CSV dictionary.

**Syntax:** `^csvKey[rowKey][columnKey]`

**Examples:**
```
// Load CSV data first (via preload anim or similar)
// Then reference cells:
Text(text=^mapData[level1][name])
Circle(x=^positions[player][xPos] y=^positions[player][yPos])
```

### `&` JSON Variable References

Navigates and extracts values from JSON objects stored in global variables.

**Syntax:** `&jsonKey.property.path`

**Examples:**
```
// Set JSON data
set(userData='{"name":{"first":"Alice","last":"Smith"},"age":30}')

// Access nested properties
Text(text=&userData.name.first)
Text(text=&userData.age)
```

### `*` Data.txt Path

Prefix for file paths that resolve to the save folder location. Used primarily with file operations.

**Note:** Path is defined in constants as `SAVES_PATH`

## Reserved Global Variable Keys

Special global variable keys that trigger system operations when set via `set()` command:

### Canvas Control

```
set(canvas.bg=blue)           // Set canvas background color
set(canvas.scene=nextLevel)   // Queue scene transition to 'nextLevel'
set(canvas.freeze=true)       // Freeze canvas rendering (true/false)
set(canvas.report=objectId)   // Generate debug report for specific object
set(canvas.tracker=report)    // Generate usage report
set(canvas.tracker=clear)     // Clear usage tracking data
```

**Deprecated (may be removed):**
```
set(canvas.save=true)   // Trigger game save operation
set(canvas.load=true)   // Trigger game load operation
```

### Asset Registration

```
set(img.myImage=path/to/image.png)              // Register image asset
set(sheet.mySheet=path/to/sheet.png)            // Register sprite sheet
```

### Local Variable Assignment

```
set(local.objectId.propertyName=value)  // Set local variable on specific object
```

**Example:**
```
set(local.player.health=100)
set(local.enemy.speed=5)
```

## Global Variable Association Prefixes

These prefixes create associations between global variables and events or prototypes:

### `!` Variable Event

Associates a global variable with an event that fires when the variable changes.

**Syntax:** `set(!variableName=eventId)`

**Example:**
```
set(!playerHealth=healthChanged)
// Now whenever 'playerHealth' changes, 'healthChanged' event fires
```

### `+` Variable Prototype

Associates a global variable with a prototype that gets created when the variable changes.

**Syntax:** `set(+variableName=prototypeId)`

**Example:**
```
set(+enemySpawned=spawnEffect)
// When 'enemySpawned' changes, the 'spawnEffect' prototype is created
```

## Object Functor Prefixes

Special prefixes used in object functor names to modify object creation behavior:

### `_()` Empty Object / Container

Creates an empty container object with no visual representation. Useful as a parent for grouping objects.

**Aliases:** `empty()`, `container()`, `cont()`

**Example:**
```
_(id=group x=100 y=100
  Circle(x=0 y=0 radius=20)
  Circle(x=50 y=0 radius=20)
)
```

### `@()` Attach to Parent

Attaches animations or nested objects to the parent/creator object. Used within object definitions.

**Example:**
```
Circle(id=myCircle x=100 y=100
  @(move(x=200 d=1000))
  @(Text(text='Label' y=30))
)
```

### `?id()` Existing Object Reference

Modifies an existing object in the scene by its ID. Allows updating properties of already-created objects.

**Syntax:** `?objectId(property=value)`

**Example:**
```
// Create object
Circle(id=myCircle x=100 y=100)

// Later, modify it
?myCircle(x=200 color=red)
```

### `$id()` Prototype Reference

Creates object(s) from a stored prototype definition.

**Syntax:**
- `$prototypeId()` - Create single instance
- `$prototypeId*count()` - Create multiple copies

**Example:**
```
// Define prototype
set(enemyDef=Circle(radius=20 color=red))

// Create instances
$enemyDef(x=100 y=100)
$enemyDef*5(y=200)  // Create 5 copies
```

**Prototype Variables:** Use `[key=value]` syntax to pass variables to prototypes

```
$enemyDef(text=[name=$playerName] x=100)
```

### `@$id()` Prototype with Creator Transform

Creates a prototype using the creator/parent object's transform properties.

**Note:** This is mentioned in the original docs but implementation details need verification.

## Processing Order

When a property value is processed, prefixes are evaluated in this order:

1. CSV references (`^`)
2. JSON references (`&`)
3. Global variable references (`$`)
4. Embedded variable references (`<<`)
5. Object variable references (`%`)
6. Random notation (`~`)
