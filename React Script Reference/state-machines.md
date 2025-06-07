# State Machines and Behavior Trees

The **State** object is a special type of object used to create state machine and behavior tree constructs.  

## State Machine Construct

A state machine is a construct that applies certain behavior to an object based on its current state.  When the state changes, the behavior changes.

Every object maintains a list of anims that are updated each animation frame. The State object is an invisible object that defers its anim's effects to its parent object.  Only one of the State objects contained by a parent object is active at a time. By changing state, a different set of anims will affect the parent object.

State is changed by setting the state property in the parent object to the ID of the new state. The first state will run by default.  

In this example, notice that the color change is performed once when a state changes, but the rotate action runs indefinitely until the state is changed again.  Both are important qualities of state machines: transitioning between states and animating in a state until it changes.  In games, it can bring a character to life and give it varied abilites.

```script
// state machine construct: rotating rectangle
Rect(
    // default state
    State(id=clockwise
        // change the parent rectangle to green
        update(color=green)

        // rotate the parent clockwise 
        rotate(velocity=45 infinite) 

        // after 5 secs, change to the counter-clockwise state
        update(wait=5000 state=counterClockwise)
    )

    // counter-clockwise state
    State(id=counterClockwise
        // change the parent rectangle to red
        update(color=red)

        // rotate the parent counter-clockwise 
        rotate(velocity=-45 infinite) 

        // after 5 secs, change to the clockwise state
        update(wait=5000 state=clockwise)
    )
)

```

## Behavior Tree Construct

A behavior tree is an animated decision tree that uses a "blackboard" associated with an object to check for changes in conditions.  The tree will follow the same decision path each animation frame until conditions change.

The [if](./anims.md#if) anim is used to monitor conditions and change state as needed.  This creates a behavior tree construct that checks conditions each animation frame.

In this example, the **if** anim is checking the value of a local variable called "status".  It is necessary to use local variables as a "blackboard" so that each object holds its own unique state.  The **if** anim's **reset** flag is set so that the anim will reset when done and continue to monitor for the variable to change again.

The default idle state does nothing so the **if** anim can decide the initial state.

```script
// behavior tree construct: patrolling oval
Oval(id=testobj local='status=normal' w=50 h=70
    // when status changes to normal, home state is set
    // when status changes to a value other than normal, alert state is set
    if('status=normal' true=home false=alert reset local)

    // default state
    State(id=idle)

    // resets to home position
    State(id=home 
        // set the parent oval green
        update(color=green)

        // go to the home position
        lerp(x=0 y=0 h=70 dur=2000 block)

        // change to the patrol state when done resetting
        update(state=patrol) 
    )

    // patrols the area
    State(id=patrol 
        // move back and forth once
        lerp(x=300 dur=3000 count=2 osc block) 

        // change to the rest state when done patroling
        update(state=rest) 
    )

    // resting
    State(id=rest 
        // simulate breathing
        lerp(h=100 count=4 osc block)

        // change to the patrol state when done reseting
        update(state=patrol) 
    )

    // alert (bounce)
    State(id=alert 
        // set the parent oval red
        update(color=red)

        // bounce up and down until the state is changed 
        lerp(y=-50 dur=250 inf osc easing=out) 
    )
)

// test buttons
// using the special "local." variable prefix to access 
// the test object's local variables

Text("alert" id=alert y=200 w=150 bg=#200
    // set the test object's status to "alert"
    set(in=click.alert local.testobj.status=alert reset)
)

Text("normal" id=normal y=250 w=150 bg=#020
    // set the test object's status to "normal"
    set(in=click.normal local.testobj.status=normal reset)
)
```

### If Anim

The **if** anim has multiple uses including setting state.  The **if** anim can also be used to [make prototypes](./prototypes.md#if-anim).
