# Anims

Anims perform specific actions, usually affecting the [object](./objects.md) they are attached to. All anims inherit the capabilities of the base anim in addition to their specific function. Multiple anims can be attached to a display object to give it their combined capabilities.

## Animation Categories

The animation system is organized into four main categories:

### [Core Anims](./core-anims.md)
Fundamental animations that provide basic object manipulation and control:
- `anim` / `listen` - Base animation class with core functionality
- `anchor` - Sets anchor points for objects
- `atlas` - Handles sprite atlas operations
- `destroy` - Destroys objects
- `fetch` - Fetches data from URLs
- `hover` - Handles mouse hover interactions
- `lerp` / `fade` / `tween` - Smooth property transitions
- `lerppath` - Animates objects along paths
- `load` - Loads and executes script files
- `local` - Creates local scope for variables
- `make` - Creates objects from prototypes
- `move` - Moves objects with velocity
- `rotate` - Rotates objects
- `set` - Sets variables
- `update` - Updates object properties

### [Utility Anims](./util-anims.md)
Utility functions for logic control and data manipulation:
- `add` / `inc` - Mathematical operations
- `arrange` - Arranges child objects in layouts
- `audio` - Audio playback
- `call` - Function calls
- `compare` - Conditional logic and comparisons
- `dovetail` - Synchronizes multiple animations
- `filter` / `copy` - Data filtering and copying
- `if` - Conditional logic
- `log` - Logging and debugging
- `preload` - Asset preloading
- `report` - Status reporting

### [Extra Anims](./extra-anims.md)
Advanced functionality for specialized operations:
- `assert` - Assertion testing
- `browse` - Browser window management
- `cookie` - Browser cookie operations
- `deck` - Card deck operations
- `diff` - Difference calculations
- `download` - File downloads
- `fence` - Boundary detection
- `format` - Text formatting
- `lint` - Code validation
- `pixel` - Pixel manipulation
- `pixelate` - Pixelation effects
- `save` - Save operations
- `scroll` - Scrolling controls
- `separate` - Separates objects or data
- `top` - Top-level operations

### [LLM Anims](./llm-anims.md)
AI-powered animations for language model integration:
- `embedding` - Vector embeddings
- `prompt` / `llm` - LLM prompt execution
- `rag` - Retrieval Augmented Generation
- `test` - AI-powered testing
- `validate` - AI validation
