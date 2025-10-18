# Compare Anim

The **compare** anim performs conditional comparisons between two variables and responds based on the result. It can compare strings or numbers using various operators, and can trigger different behaviors depending on whether the comparison evaluates to true or false.

## Command ID

`compare`

## Core Functionality

The compare anim evaluates a comparison between two variables and executes different actions based on the result. Unlike most anims that complete and stop, the compare anim's lifecycle depends on several factors:

- **Comparison result**: By default, the anim stops when the comparison is true
- **Repeat flag**: When `repeat` is set, the anim continues evaluating every frame
- **Action responses**: When `true` or `false` values are provided, the anim behavior changes

## Animation Lifecycle

The compare anim's completion behavior follows these rules:

1. **No true/false values set**: The anim completes (`done=true`) when the comparison is true
2. **True/false values set**: The anim completes after taking action
3. **Repeat enabled**: The anim never completes and evaluates every frame
4. **Result-based completion**: If false value is empty and comparison is false, the anim stays alive (allows waiting for true condition)

## Properties

### Comparison Properties

| Property | Aliases | Type | Description |
|----------|---------|------|-------------|
| `key1` | `key` | string | First variable to compare |
| `key2` | `match` | string | Second variable to compare |
| `operator` | `text` | string | Comparison operator (default: `=`) |
| `type` | - | string | Comparison type: `str` or `num` (default: `str`) |
| `numeric` | `num`, `number` | boolean | Flag to set type to `num` |

### Supported Operators

**String comparisons:**

- `=` or `==` - Equals
- `!=` - Not equals

**Numeric comparisons:**

- `=` or `==` - Equals
- `!=` - Not equals
- `<` - Less than
- `>` - Greater than
- `<=` - Less than or equal
- `>=` - Greater than or equal

### Response Properties

| Property | Aliases | Type | Description |
|----------|---------|------|-------------|
| `trueValue` | `true`, `yes` | string | Action when comparison is true |
| `falseValue` | `false`, `no` | string | Action when comparison is false |
| `target` | `t` | string | ID of target object (default: self) |

### Behavior Properties

| Property | Aliases | Type | Description |
|----------|---------|------|-------------|
| `local` | - | boolean | Use local variables instead of global |
| `repeat` | `rep`, `inf`, `infinity`, `infinite` | boolean | Keep evaluating every frame |
| `literal` | `lit` | boolean | Treat key2 as a literal value instead of a variable name |

## Response Actions

The `true` and `false` properties determine what happens when the comparison succeeds or fails:

### 1. State Change

If the target object has states, set the state by name or index:

```script
compare(key1=status key2=ready true=0 false=1)
compare(key1=mode key2=active true=running false=idle)
```

### 2. Prototype Creation

Create an object from a prototype definition:

```script
compare(key1=score ">=" key2=100
  true="Text('You win!' color=green)"
  false="Text('Keep trying')"
)
```

### 3. Prototype with Transform

Prefix with `+` to force prototype creation even if target has states:

```script
compare(key1=health "<=" key2=0
  true="+Circle(color=red move(dy=-100 des))"
)
```

### 4. Add Anim to Target

Use [@ notation](./prototypes.md#-notation) to add an anim to the parent object:

```script
compare(key1=power key2=max
  true="@(update(text='FULL'))"
  false="@(update(text='$power'))"
)
```

## Examples

### Example 1: Prototype Creation (sample-compare.txt)

Test a variable and create different text messages based on the result:

```script
// Variables
Def(
  counter=5
  limitMax=5
  limitMin=0
)

// Test logic in a prototype
_(id=protoTest
  compare(key=counter key2=limitMax
    true="Text('IS at maximum' y=-150)"
    false="Text('NOT at maximum' y=-150)"
    destroy
  )
)
```

**Behavior**: The compare anim evaluates once. If counter equals limitMax, it creates a "IS at maximum" text. Otherwise, it creates a "NOT at maximum" text. After taking action, the anim completes and the container destroys itself.

### Example 2: State Machine Counter (sample-compare-2.txt)

Create a counter that bounces between min and max values using states:

```script
// Variables
Def(
  counter=0
  limitMax=5
  limitMin=0
)

// Text display with state machine
Text(text='0' x=-300 y=-50 size=80
  State(id=up
    // Switch to down state when reaching max
    compare(key1=counter key2=limitMax ">=" true=down type=num)

    // Increment counter
    add(wait=1000 key=counter v=1 block reset)

    // Update display
    update(text=$counter reset)
  )

  State(id=down
    // Switch to up state when reaching min
    compare(key1=counter key2=limitMin "<=" true=up type=num)

    // Decrement counter
    add(wait=1000 key=counter v=-1 block reset)

    // Update display
    update(text=$counter reset)
  )
)
```

**Behavior**: The compare anim in each state evaluates continuously. When the condition is met, it changes the state. The anim completes after the state change, but the new state's compare anim takes over.

### Example 3: Continuous Monitoring (sample-compare-3.txt)

Monitor a variable and update object state in real-time:

```script
// Definitions
Def(
  status.ready=ready
  status.busy=busy

  _(id=tester
    set(wait=3000 local.testobj.status=ready block)
    set(wait=3000 local.testobj.status=busy block)
    make(wait=5000 script=tester destroy)
  )
)

// Monitor object (invisible)
_(id=monitor
  compare(
    target=testobj
    key1=status key2=s.ready local
    true=0 false=1
    repeat
  )
)

// Spinning rectangle with states
Rect(id=testobj local='status=idle s.ready=ready s.busy=busy' w=200
  State(
    anim(make=ready)
    rotate(vel=-45 inf)
  )
  State(
    anim(make=busy)
    rotate(vel=180 inf)
  )
)

// Start the tester
$tester()
```

**Behavior**: The monitor's compare anim runs every frame (`repeat=true`), checking testobj's local status variable. When status equals "ready", it sets state to 0 (first state). When not equal, it sets state to 1 (second state). The anim never completes because `repeat` is enabled.

## Usage Patterns

### One-Shot Test

```script
Def(
  value=4
  threshold=3
)
_(
  compare(key1=value ">" key2=threshold
    true="Circle(color=green)"
  )
)
```

Evaluates once, creates object, then completes.

### Continuous Monitor

```script
compare(key1=status literal key2=active
  true=running false=idle
  repeat
)
```

Evaluates every frame, switches states as needed, never completes.

### Behavior Tree Branch

```script
State(id=patrol
  compare(key1=distance "<" key2=alertRange true=alert)
  move(patrol pattern...)
)
```

Evaluates continuously, switches state when condition met, then completes.

### Waiting for Condition

```script
compare(key1=ready literal key2=true true="+startGame")
```

Stays alive until condition is true, then creates prototype and completes.

## Local vs Global Variables

- **Global variables**: Accessible across all objects via `globalVariables` registry
- **Local variables**: Stored on individual objects, accessed with `local` flag

```script
// Global comparison
compare(key1=score ">" key2=highScore)

// Local comparison
compare(local key1=health key2=maxHealth)
```

## Literal Values

By default, `key2` is treated as a variable name. Use the `literal` flag to compare against a literal string or number value:

```script
// Compare variable to literal string
compare(key1=name literal key2=Player1)

// Compare variable to literal number (string comparison)
compare(key1=status literal key2=active)

// Compare variable to another variable (default behavior)
compare(key1=name key2=playerName)
```

## Notes

- Default operator is `=` (equality)
- Default type is `str` (string comparison)
- If target is not found, uses the display object (self)
- Empty true/false values affect completion behavior
- State changes only work if target has a state machine
- Prototype creation with `+` prefix bypasses state setting
- Use `repeat` for frame-by-frame monitoring
- Numeric comparisons parse strings to integers
- Use `literal` flag to compare against constant values instead of variable names

## See Also

- [if anim](./anims.md#if--compare) - Similar conditional anim with wait property
- [State Machines](./state-machines.md) - Using compare for state transitions
- [Prototypes](./prototypes.md) - Creating objects from compare results
- [@ Notation](./prototypes.md#-notation) - Adding anims to target objects
