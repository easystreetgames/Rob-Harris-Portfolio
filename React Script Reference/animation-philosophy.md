# Animation Philosophy: The Alchemy System

This document explains the foundational concepts behind the ESG animation system and how to practice **animation alchemy** - combining existing animations to create sophisticated effects without building new ones.

## Table of Contents

1. [The Foundation: Anim Base Class](#the-foundation-anim-base-class)
2. [The Core Building Blocks](#the-core-building-blocks)
3. [The Alchemy Principle](#the-alchemy-principle)
4. [Essential Combination Patterns](#essential-combination-patterns)
5. [When You Actually Need a New Anim](#when-you-actually-need-a-new-anim)

## The Foundation: Anim Base Class

**Every animation inherits from [Anim](./core-anims.md#anim--listen)**, which provides critical universal abilities that work across all animation types:

### Universal Properties

- **make** (aliases: `then`, `proto`): Create [prototypes](./prototypes.md) when animation completes - the most powerful chaining mechanism
- **in** (alias: `inEvent`): Event that activates this animation - enables event-driven behavior
- **out** (alias: `outEvent`): Event to trigger when animation completes - enables cascading behaviors
- **set**: Set global variables upon completion - integrates with the variable system
- **block** (alias: `b`): Control animation sequencing - blocks subsequent anims until complete
- **destroy** (alias: `des`): Destroy the parent GameObject when animation completes
- **removeSelf** (aliases: `remove`, `rem`): Remove the animation when it completes (default: true)
- **wait**: Delay in milliseconds before animation starts
- **reset**: Restart the animation when it completes - enables infinite loops and repeating effects (see [reset.md](./reset.md))

Since all anims inherit these properties, *every animation can spawn new objects, fire events, set variables, and restart*. This makes even simple anims like `lerp` capable of complex behaviors.

## The Core Building Blocks

Rather than creating hundreds of specialized animations, the system provides versatile building blocks that combine to produce sophisticated results. See detailed documentation in the linked references.

### 1. Prototypes - The Reusability System
Any script can be a [prototype](./prototypes.md):
- Created via `make` property (inherited by all anims), `Proto()` command, `$` notation, or [make](./core-anims.md#make) anim
- Support randomness, property overrides, and parent/child relationships
- The key to spawning, chaining, and particle effects

### 2. lerp - Universal Property Interpolation
[lerp](./core-anims.md#lerp--tween) (aliases: `tween`, `fade`) animates any property smoothly:
- **Properties**: `x`, `y`, `w/width`, `h/height`, `angle`, `frame`, `alpha`, `scale`, `color`
- **Easing**: `in`, `out`, `in-out`, `bounce`, `elastic`, `back`, `expo`, `sine`, `cubic`
- **Repetition**: `count` (finite or infinite with `-1`), `reverse/osc` for oscillation
- **Duration**: `duration/dur` in milliseconds
- **Target**: `target` to animate other objects

### 3. set - Variable Manipulation
[set](./core-anims.md#set) manages state through variables:
- Sets global or local variables using key=value pairs
- Resolves random notation (`~min<max`) and property references (`$var`)
- Foundation for state management and data flow
- Works with special keys like `canvas.bg`, `local.objectId.prop`

### 4. update - Instant Property Changes
[update](./core-anims.md#update) modifies properties instantly:
- Updates any object property using key=value pairs
- Can target other objects via `target` property
- Use with [reset](./reset.md) for continuously updating displays

### 5. compare - Conditional Logic Engine
[compare](./compare-anim.md) enables decision-making:
- Compares two variables with operators: `=`, `!=`, `<`, `>`, `<=`, `>=`
- Actions: `true`/`false` values can change state or create prototypes
- `repeat` flag: continuous frame-by-frame monitoring
- `local` flag: per-object behavior trees
- Powers [behavior trees](./state-machines.md#behavior-tree-construct)

### 6. if - Simplified Conditional
[if](./util-anims.md#if--compare) provides simpler syntax:
- Single test string: `test='varName=value'`
- Actions: `true`/`false` for state changes or prototype creation
- Use with [reset](./reset.md) for continuous monitoring

### 7. make - Prototype Spawning Over Time
[make](./core-anims.md#make) creates objects repeatedly:
- `script`: prototype ID or inline definition
- `count`: number of instances to create
- `interval`: delay between instances for particle trails/spawners
- Different from `make` property: this spawns over time

### 8. move - Velocity-Based Movement
[move](./core-anims.md#move) provides continuous motion:
- `vx`/`vy`: velocity in pixels per second
- `velocity/v`: forward velocity based on rotation
- `duration/dur`: how long to move
- Use `inf` or [reset](./reset.md) for continuous movement

### 9. States - Behavior Switching
[State objects](./state-machines.md) enable [state machines](./state-machines.md#state-machine-construct):
- Defer anims to parent object
- Only one active at a time
- Change via `state` property or compare/if results

### 10. Layout & Positioning
Specialized positioning tools:
- [anchor](./core-anims.md#anchor): Canvas-relative positioning
- [arrange](./util-anims.md#arrange): Auto-layout children
- [dovetail](./util-anims.md#dovetail): Align to other objects

### 11. Special Purpose Anims
See category documentation for full details:
- [Core Anims](./core-anims.md): `rotate`, `hover`, `lerppath`, `atlas`, `load`, `destroy`
- [Utility Anims](./util-anims.md): `add`, `mult`, `local`, `fetch`, `filter`, `audio`, `call`
- [Extra Anims](./extra-anims.md): `separate`, `pixelate`, `fence`, `deck`, `browse`, `cookie`
- [LLM Anims](./llm-anims.md): `prompt`, `rag`, `embedding`

## The Alchemy Principle

Instead of creating 100 specialized anims, combine these building blocks:

- **lerp + make**: Animate an object, spawn new objects when done
- **compare + make**: Conditional prototype creation (behavior trees)
- **compare + repeat**: Frame-by-frame monitoring for reactive behaviors
- **set + compare**: State machines via variable manipulation and testing
- **update + reset**: Continuously update properties (counters, displays)
- **add + reset**: Counters and timers that increment continuously
- **Anim.make property**: Any anim can spawn prototypes when complete
- **make anim + interval**: Spawning over time for particle trails
- **hover + in/out events**: Interactive state transitions
- **local + compare + local flag**: Per-object behavior trees
- **anchor + arrange**: Responsive layouts
- **dovetail + inf**: Dynamic alignment during animations
- **lerppath + bezier**: Complex curved motion
- **separate + proto**: Text explosion effects
- **call + update**: Bridge to TypeScript for custom physics

For practical examples, see [animation-recipes.md](./animation-recipes.md).

## Essential Combination Patterns

### Smooth Animations
- `lerp` + `count=-1` = Infinite animation
- `lerp` + `reverse` + `count` = Oscillation
- `lerp` + `easing` = Natural motion curves
- [lerppath](./core-anims.md#lerppath) + `bezier` = Smooth curved paths

### Spawning & Particles
- Any anim + `make` property = Spawn on completion
- [make](./core-anims.md#make) anim + `interval` + `count` = Particle trails
- [separate](./extra-anims.md#separate) + `proto` = Text explosion effects
- [pixelate](./extra-anims.md#pixelate) + `proto` = Image explosion effects

### Interactivity
- [hover](./core-anims.md#hover) + `in`/`out` events + States = Button behaviors
- `hover` + `once` + `make` = Click to spawn
- [compare](./compare-anim.md) + `local` + `repeat` = Per-object reactivity

### State Machines
- States + `compare` + `repeat` = [Behavior trees](./state-machines.md#behavior-tree-construct)
- States + `if` + `reset` = Continuous condition monitoring
- `update` + `state=stateName` = Manual state changes
- `compare` true/false = Automatic state transitions

### Counters & Timers
- [add](./util-anims.md#add) + `reset` + `wait` + `block` = Incrementing counter
- `add` + `time` + `prefix` = Timer display
- `update` + `text=$var` + `reset` = Display updates

### Layouts & Positioning
- [anchor](./core-anims.md#anchor) + `offsetX/Y` = Canvas-relative placement
- [arrange](./util-anims.md#arrange) + `direction` + `gap` = Auto-layout
- [dovetail](./util-anims.md#dovetail) + `target` + `anchor` = Relative positioning
- `dovetail` + `inf` = Dynamic alignment during animations

### Variables & Data Flow
- [set](./core-anims.md#set) key=value = Global variables
- [local](./core-anims.md#local) key=value = Object variables
- [fetch](./core-anims.md#fetch) + `property` = Transfer var to property
- `set` local.objId.var = Set other object's local var
- See [prefixes.md](./prefixes.md) for variable notation

### Complex Behaviors
- `compare` + `repeat` + `true/false` = Reactive behaviors
- [call](./util-anims.md#call) + custom function = Physics, pathfinding, AI
- `update` + `call` + `reset` = Frame-by-frame custom logic
- States + nested `compare` = Hierarchical behavior trees

## When You Actually Need a New Anim

Before creating a new anim, ask yourself:

### 1. Can I use lerp?
- Smooth property transitions? → Yes
- Multiple properties at once? → Yes
- Easing, repetition, oscillation? → Yes
- Paths and curves? → Use [lerppath](./core-anims.md#lerppath)

### 2. Can I use compare or if?
- Conditional logic? → Yes
- State transitions? → Yes
- Continuous monitoring? → Yes (with `repeat`)
- Per-object decisions? → Yes (with `local`)

### 3. Can I use the make property?
- Spawn objects after animation? → Yes
- Chain behaviors? → Yes
- Particle effects? → Yes (combine with [make](./core-anims.md#make) anim)

### 4. Can I use call + TypeScript?
- Complex math or physics? → Yes
- Custom logic? → Yes
- Integration with libraries? → Yes

### 5. Can I combine multiple existing anims?
- Most effects? → Yes
- Check [animation-recipes.md](./animation-recipes.md)

### Create a new anim only if:
- Requires specialized rendering (mesh warping, shaders)
- Needs tight integration with engine internals
- Performance-critical (particle systems for 1000+ particles)
- Would be extremely tedious to replicate with combinations

## Quick Reference by Use Case

**Movement:**
- Smooth: [lerp](./core-anims.md#lerp--tween) x/y/dx/dy
- Velocity: [move](./core-anims.md#move) vx/vy
- Paths: [lerppath](./core-anims.md#lerppath) with bezier
- Rotation: [lerp](./core-anims.md#lerp--tween) angle or [rotate](./core-anims.md#rotate) vel

**Visuals:**
- Fade: [lerp](./core-anims.md#lerp--tween) alpha
- Scale: [lerp](./core-anims.md#lerp--tween) scale
- Color: [lerp](./core-anims.md#lerp--tween) color
- Frames: [lerp](./core-anims.md#lerp--tween) frame

**Layout:**
- Canvas edges: [anchor](./core-anims.md#anchor)
- Children: [arrange](./util-anims.md#arrange)
- Relative: [dovetail](./util-anims.md#dovetail)

**Logic:**
- Conditions: [compare](./compare-anim.md), [if](./util-anims.md#if--compare)
- Variables: [set](./core-anims.md#set), [add](./util-anims.md#add), [mult](./util-anims.md#mult)
- Properties: [update](./core-anims.md#update), [fetch](./core-anims.md#fetch)
- States: [State objects](./state-machines.md)

**Spawning:**
- On complete: `make` property
- Over time: [make](./core-anims.md#make) anim
- Text: [separate](./extra-anims.md#separate)
- Images: [pixelate](./extra-anims.md#pixelate)

**Interaction:**
- Mouse: [hover](./core-anims.md#hover)
- Events: `in`/`out` properties
- Custom: [call](./util-anims.md#call)

**Assets:**
- Scripts: [load](./core-anims.md#load)
- Sprites: [atlas](./core-anims.md#atlas)
- Audio: [audio](./util-anims.md#audio)

## Related Documentation

- [Animation Recipes](./animation-recipes.md) - Practical combination patterns
- [Prototypes](./prototypes.md) - Object creation and reusability
- [State Machines](./state-machines.md) - Behavior switching
- [Reset Property](./reset.md) - Infinite loops and continuous behaviors
- [Compare Anim](./compare-anim.md) - Conditional logic
- [Prefixes](./prefixes.md) - Variable and value notation
- [Core Anims](./core-anims.md) - Fundamental animations
- [Utility Anims](./util-anims.md) - Logic and data manipulation
- [Extra Anims](./extra-anims.md) - Specialized operations
- [LLM Anims](./llm-anims.md) - AI integration
