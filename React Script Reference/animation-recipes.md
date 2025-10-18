# Animation Recipes: Combining Anims for Common Effects

This document provides practical recipes for combining existing animations to create sophisticated effects. For the underlying philosophy and principles, see [animation-philosophy.md](./animation-philosophy.md).

## Table of Contents

1. [Movement & Animation Recipes](#movement--animation-recipes)
2. [Visual Effects Recipes](#visual-effects-recipes)
3. [UI & Interaction Recipes](#ui--interaction-recipes)
4. [Advanced Combination Patterns](#advanced-combination-patterns)
5. [State Machine Patterns](#state-machine-patterns)

## Movement & Animation Recipes

### Pulse Effect
**Combination**: [lerp](./core-anims.md#lerp--tween) scale + `reverse` + `count`

```javascript
// Pulsing heart icon
_( lerp(scale=1.2 d=500 reverse count=-1 ease='in-out') )
```

### Bounce Effect
**Combination**: [lerp](./core-anims.md#lerp--tween) y + `bounce` easing

```javascript
// Bouncing ball - single bounce
_( lerp(dy=-200 d=800 ease='bounce') lerp(dy=200 d=400) )

// Continuous bouncing with diminishing returns
_( lerp(dy=-200 d=600 ease='bounce' reverse count=-1) )
```

### Shake/Vibration
**Combination**: [lerp](./core-anims.md#lerp--tween) x + `reverse` + fast timing

```javascript
// Camera shake
_( lerp(dx=10 d=50 reverse count=5) )

// Earthquake effect
_( lerp(dx=-15 dy=8 d=40 reverse count=10) )
```

### Orbit/Circle Motion
**Combination**: [update](./core-anims.md#update) + [call](./util-anims.md#call) with sin/cos math

```javascript
// Register orbit function
CallAnim.registerFunction('orbit', (obj, radius, speed) => {
  const angle = (Date.now() * speed) % (Math.PI * 2);
  obj.x = centerX + Math.cos(angle) * radius;
  obj.y = centerY + Math.sin(angle) * radius;
});

// Apply orbit with reset for continuous movement
_( update(call('orbit' params='100,0.001') reset) )
```

### Elastic Snap
**Combination**: [lerp](./core-anims.md#lerp--tween) x/y + `back` easing

```javascript
// Rubber band effect to position
_( lerp(x=500 y=300 d=1200 ease='back') )
```

### Wobble/Oscillate
**Combination**: [lerp](./core-anims.md#lerp--tween) angle + `reverse` + `sine` easing

```javascript
// Pendulum swing
_( lerp(angle=45 d=800 reverse count=-1 ease='sine') )
```

### Continuous Spin
**Combination**: [rotate](./core-anims.md#rotate) or [lerp](./core-anims.md#lerp--tween) angle + high count

```javascript
// Infinite rotation
_( lerp(angle=360 d=2000 count=-1) )

// Or with rotate anim
_( rotate(velocity=180 inf) )
```

## Visual Effects Recipes

### Fade In/Out
**Combination**: [lerp](./core-anims.md#lerp--tween) alpha

```javascript
// Fade in
_( lerp(alpha=1 d=500) )

// Fade out
_( lerp(alpha=0 d=500) )

// Crossfade (on two objects)
_( lerp(alpha=0 d=800) )  // on old object
_( lerp(alpha=1 d=800) )  // on new object
```

### Color Pulse/Glow
**Combination**: [lerp](./core-anims.md#lerp--tween) color + `reverse`

```javascript
// Glowing red warning
_( lerp(color='#ff0000' d=600 reverse count=-1) )
```

### Scale Pulse (Attention Grabber)
**Combination**: [lerp](./core-anims.md#lerp--tween) scale + `back` easing

```javascript
// Pop effect
_( lerp(scale=1.3 d=300 ease='back') lerp(scale=1.0 d=200) )
```

### Slide Transitions
**Combination**: [lerp](./core-anims.md#lerp--tween) x/y with off-screen start

```javascript
// Slide in from left
_( lerp(x=400 d=600 ease='out') )  // assuming starts at x=-200

// Slide out to right
_( lerp(x=1200 d=600 ease='in') )

// Slide up accordion
_( lerp(h=0 alpha=0 d=400) )
```

### Card Flip
**Combination**: [lerp](./core-anims.md#lerp--tween) scaleX + [if](./util-anims.md#if--compare) or state change at midpoint

```javascript
// 3D-style flip using scaleX
_( lerp(sx=0 d=200) if(test='$frame=0' true='flip' false='') lerp(sx=1 d=200) )

// Or with states (see state-machines.md)
_( lerp(sx=0 d=200) update(state=flipped d=0) lerp(sx=1 d=200) )
```

### Loading Spinner
**Combination**: [lerp](./core-anims.md#lerp--tween) angle continuous

```javascript
// Rotating spinner
_( lerp(angle=360 d=1000 count=-1) )
```

### Progress Bar
**Combination**: [lerp](./core-anims.md#lerp--tween) width + [set](./core-anims.md#set)

```javascript
// Animated progress
_( set(progress=75) lerp(w=$progress d=500 ease='out') )
```

### Particle Burst
**Combination**: [make](./core-anims.md#make) anim + [lerp](./core-anims.md#lerp--tween) with random positions

See [prototypes.md](./prototypes.md) for detailed prototype creation patterns.

```javascript
// Create multiple particles over time
_(
  make(count=10 interval=100
    "Circle(radius=5 color='#ff0'
      lerp(dx=~-200<200 dy=~-200<200 alpha=0 d=1000 destroy)
    )"
  )
)
```

### Screen Shake
**Combination**: Camera object with [lerp](./core-anims.md#lerp--tween) x/y

```javascript
// Apply to camera/scene container
_( lerp(dx=~-10<10 dy=~-10<10 d=50 count=8) )
```

### Zoom Effect
**Combination**: [lerp](./core-anims.md#lerp--tween) scale on container + x/y to center

```javascript
// Zoom into point
_( lerp(scale=2.0 x=400 y=300 d=800 ease='in-out') )
```

## UI & Interaction Recipes

### Button Press Effect
**Combination**: [hover](./core-anims.md#hover) + [lerp](./core-anims.md#lerp--tween) scale with events

See [state-machines.md](./state-machines.md) for state management patterns.

```javascript
// Button object
Rect(w=120 h=40
  hover(in='pressed' out='released')

  State(id=pressed
    lerp(scale=0.95 d=100)
  )

  State(id=released
    lerp(scale=1.0 d=100)
  )
)
```

### Tooltip on Hover
**Combination**: [hover](./core-anims.md#hover) + [make](./core-anims.md#make) property + [lerp](./core-anims.md#lerp--tween) alpha

```javascript
Rect(w=100 h=30
  hover(in='showTooltip' out='hideTooltip')

  // Tooltip appears
  State(id=showTooltip
    anim(make="Text(text='Info' alpha=0 id='tip' lerp(alpha=1 d=200))")
  )

  State(id=hideTooltip
    destroy(target='tip')
  )
)
```

### Highlight on Hover
**Combination**: [hover](./core-anims.md#hover) + [lerp](./core-anims.md#lerp--tween) color/alpha

```javascript
Rect(color='#333'
  hover(in='highlight' out='unhighlight')

  State(id=highlight
    lerp(color='#666' d=200)
  )

  State(id=unhighlight
    lerp(color='#333' d=200)
  )
)
```

### Drag & Drop Pattern
**Combination**: [update](./core-anims.md#update) + [call](./util-anims.md#call) with mouse position tracking

```javascript
// Register drag handlers
CallAnim.registerFunction('startDrag', (obj) => {
  obj.setVar('dragging', 'true');
  obj.setVar('offsetX', mouseX - obj.x);
  obj.setVar('offsetY', mouseY - obj.y);
});

CallAnim.registerFunction('updateDrag', (obj) => {
  if (obj.getVar('dragging') === 'true') {
    obj.x = mouseX - parseInt(obj.getVar('offsetX'));
    obj.y = mouseY - parseInt(obj.getVar('offsetY'));
  }
});

// Apply to object
Circle(radius=30
  hover(in='startDrag')
  update(call('updateDrag') reset)
)
```

### Toggle Switch Animation
**Combination**: [if](./util-anims.md#if--compare) + [lerp](./core-anims.md#lerp--tween) x + state

See [state-machines.md](./state-machines.md) for more complex state examples.

```javascript
Rect(w=60 h=30 color='#ccc'
  Circle(id='knob' radius=12 x=15 y=15)
  hover(in='toggle')

  State(id=toggle
    if(test='$state=0' true='on' false='off')
  )

  State(id=on
    set(state=1)
    lerp(target='knob' x=45 d=200 ease='out')
    lerp(color='#4CAF50' d=200)
  )

  State(id=off
    set(state=0)
    lerp(target='knob' x=15 d=200 ease='out')
    lerp(color='#ccc' d=200)
  )
)
```

### Accordion Expand/Collapse
**Combination**: [if](./util-anims.md#if--compare) + [lerp](./core-anims.md#lerp--tween) height + state

```javascript
Container(h=50
  hover(in='toggleAccordion')

  State(id=toggleAccordion
    if(test='$expanded=1' true='collapse' false='expand')
  )

  State(id=expand
    set(expanded=1)
    lerp(h=300 d=400 ease='out')
  )

  State(id=collapse
    set(expanded=0)
    lerp(h=50 d=400 ease='in')
  )
)
```

## Advanced Combination Patterns

### Physics Simulation
**Combination**: [update](./core-anims.md#update) + [call](./util-anims.md#call) with velocity/gravity

```javascript
// Register physics update
CallAnim.registerFunction('gravity', (obj) => {
  let vy = parseFloat(obj.getVar('vy') || '0');
  vy += 0.5; // gravity
  obj.y += vy;
  obj.setVar('vy', vy.toString());

  // Bounce on ground
  if (obj.y > 500) {
    obj.y = 500;
    obj.setVar('vy', (vy * -0.8).toString());
  }
});

Circle(radius=20 y=100
  update(call('gravity') reset)
)
```

### Trail Effect
**Combination**: [update](./core-anims.md#update) + `make` property + [lerp](./core-anims.md#lerp--tween) alpha

```javascript
// Create fading trail
CallAnim.registerFunction('trail', (obj) => {
  if (Math.random() < 0.3) {
    AnimUtils.makeProto(`Circle(x=${obj.x} y=${obj.y} radius=5 alpha=1
      lerp(alpha=0 d=500 destroy)
    )`);
  }
});

Circle(radius=15
  lerp(x=700 y=400 d=3000)
  update(call('trail') reset)
)
```

### Follow/Chase Behavior
**Combination**: [update](./core-anims.md#update) + [call](./util-anims.md#call) with target tracking

```javascript
// Smooth following
CallAnim.registerFunction('follow', (obj, targetId, speed) => {
  const target = Services.animatedCanvas.getObjectById(targetId);
  if (target) {
    const dx = target.x - obj.x;
    const dy = target.y - obj.y;
    obj.x += dx * parseFloat(speed);
    obj.y += dy * parseFloat(speed);
  }
});

Circle(radius=10
  update(call('follow' params='player,0.05') reset)
)
```

### Timed Spawn System
**Combination**: [make](./core-anims.md#make) anim with interval + randomization

See [prototypes.md](./prototypes.md#the-make-anim) for detailed spawning patterns.

```javascript
// Spawn enemies every 2 seconds
Define(
  Circle(id=enemy color='red' radius=20
    move(vx=~-50<50 vy=~-50<50 dur=3000 destroy)
  )
)

_(
  make(script=enemy count=-1 interval=2000)
)
```

### Health Bar
**Combination**: [compare](./compare-anim.md) + [lerp](./core-anims.md#lerp--tween) width + color

See [compare-anim.md](./compare-anim.md) for detailed conditional logic.

```javascript
Container(
  Rect(id='bar' w=200 h=20 color='#0f0')
  compare(key1='health' op='<' key2=50 true='warning' false='healthy' repeat)

  State(id=warning
    lerp(target='bar' color='#f00' d=200)
  )

  State(id=healthy
    lerp(target='bar' color='#0f0' d=200)
  )
)

// Update health when damaged
@damaged:
  _(
    add(key='health' value=-10)
    lerp(target='bar' w=$health d=200)
  )
```

## State Machine Patterns

See [state-machines.md](./state-machines.md) for complete state machine documentation.

### Simple Two-State Toggle

```javascript
Rect(
  State(id=toggle
    if(test='$state=0' true='on' false='off' reset)
  )

  State(id=on
    set(state=1)
    lerp(color='#0f0' d=200)
  )

  State(id=off
    set(state=0)
    lerp(color='#f00' d=200)
  )
)
```

### Multi-State Cycle

```javascript
Circle(radius=30
  State(id=cycle
    if(test='$state=0' true='yellow')
    if(test='$state=1' true='red')
    if(test='$state=2' true='green')
  )

  State(id=yellow
    set(state=1)
    lerp(color='#ff0' d=200)
    anim(wait=2000 out='cycle')
  )

  State(id=red
    set(state=2)
    lerp(color='#f00' d=200)
    anim(wait=2000 out='cycle')
  )

  State(id=green
    set(state=0)
    lerp(color='#0f0' d=200)
    anim(wait=2000 out='cycle')
  )
)
```

### Conditional State Transitions (Behavior Tree)

See [state-machines.md#behavior-tree-construct](./state-machines.md#behavior-tree-construct) for detailed examples.

```javascript
Sprite(
  // Behavior tree monitoring
  compare(key1='health' op='>' key2=0 true='alive' false='dead' repeat)
  compare(key1='speed' op='>' key2=0 true='moving' false='idle' repeat local)

  State(id=alive
    if(test='moving=1' true='walk' false='stand' reset local)
  )

  State(id=walk
    lerp(frame=1 d=100 count=-1 reverse)
  )

  State(id=stand
    lerp(frame=0 d=0)
  )

  State(id=dead
    lerp(alpha=0 d=600 destroy)
  )
)
```

### Timer-Based State Changes

```javascript
Circle(color='#0ff'
  State(id=active
    lerp(scale=1.2 d=300 ease='back')
    anim(wait=5000 out='warn')
  )

  State(id=warn
    lerp(color='#f00' d=200 reverse count=5)
    anim(wait=3000 out='expired')
  )

  State(id=expired
    lerp(alpha=0 scale=0 d=300 destroy)
  )
)
```

## Related Documentation

- [Animation Philosophy](./animation-philosophy.md) - Core concepts and principles
- [Prototypes](./prototypes.md) - Object creation patterns
- [State Machines](./state-machines.md) - Behavior switching
- [Reset Property](./reset.md) - Infinite loops and continuous behaviors
- [Compare Anim](./compare-anim.md) - Conditional logic details
- [Core Anims](./core-anims.md) - lerp, move, rotate, etc.
- [Utility Anims](./util-anims.md) - if, add, call, etc.
- [Extra Anims](./extra-anims.md) - separate, pixelate, etc.
