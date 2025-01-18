# Game Architecture Notation System Specification

## 1. Overview
This document specifies a notation system for describing game architecture components and their interactions. The system provides a standardized way to represent actions, control sequences, state machines, and behavior trees.

## 2. Basic Elements

### 2.1 Actions
Actions are the fundamental building blocks of the notation system. They represent atomic operations or behaviors.

**Types:**
1. Basic Action:
```
Action1
```

2. Branching Action:
```
Decision(Action1, Action2)
```

3. Simultaneous Actions:
```
Parallel(Action1, Action2, ...)
```

**Usage Guidelines:**
- Basic actions should be atomic and clearly named
- Branching actions must specify all possible outcomes
- Parallel actions execute simultaneously
- Actions can be nested within other structures

## 3. Notation Elements

### 3.1 Control Sequences [CS]
Control sequences represent ordered series of actions that may include decision points and parallel execution.

**Syntax:**
```
[CS: Action1, Action2, Action3]
[CS: Action1, Decision(Action2, Action3)]
[CS: Action1, Parallel(Action2, Action3), Action4]
```

**Usage Guidelines:**
- Actions are executed in order from left to right
- Commas separate sequential actions
- Decision points can branch the sequence
- Parallel blocks execute their actions simultaneously
- Sequences continue after parallel blocks complete

### 3.2 State Machines [SM]
State machines represent discrete states and their associated sequences. When a state change occurs, the current state's sequence is immediately aborted and the new state's sequence is started from its beginning.

**Syntax:**
```
[SM: State1(Seq1), State2(Seq2), ...]
```

**Usage Guidelines:**
- Each state contains a sequence of actions
- When entering a state, its sequence always starts from the beginning
- The previous state's sequence is immediately terminated on state change
- No explicit transitions between states - states are switched directly
- States can be switched at any time, interrupting the current sequence

### 3.3 Behavior Trees [BT]
Behavior trees represent hierarchical decision-making structures.

**Syntax:**
```
[BT: Root(Sequence(Selector(Action1, Action2), Selector(Action3, Action4)))]
```

**Usage Guidelines:**
- Root node must be explicitly defined
- Use Sequence for sequential execution
- Use Selector for priority-based selection
- Actions can be nested to any depth
- Clear parentheses matching is essential

## 4. Example Implementation

**Game Initialization:**
```
[CS: LoadResources, Parallel(InitializeManagers, LoadUI), StartGame]
```

**Enemy Behavior:**
```
[SM: Idle(SearchTarget), Moving(UpdatePath, CheckCollision), Attacking(SelectTarget, PerformAttack)]
```

**AI Decision Making:**
```
[BT: Root(Sequence(
    Selector(CheckHealth, Retreat),
    Selector(EngageTarget, SearchArea)
))]
```

**Resource Management:**
```
[CS: CollectResource, Decision(
    ProcessResource,
    Parallel(StoreResource, UpdateUI)
)]
```

## 5. Best Practices

### 5.1 General Guidelines
1. Use clear, descriptive names for actions and states
2. Maintain consistent indentation in complex structures
3. Document decision points and conditions
4. Keep nesting depth manageable
5. Use parallel execution judiciously

### 5.2 Structure-Specific Guidelines

**Control Sequences:**
- Keep sequences focused on a single responsibility
- Use decision points for clear branching
- Document parallel execution requirements

**State Machines:**
- Define clear entry and exit conditions
- Keep state-specific sequences simple
- Document state transitions

**Behavior Trees:**
- Maintain clear hierarchy
- Use meaningful selector conditions
- Document priority orders in selectors

## 6. Testing Considerations
1. Verify sequential execution order
2. Test all decision branches
3. Validate parallel execution timing
4. Check state transition conditions
5. Test behavior tree priority handling
