# Intusic Game Kit

The game kit is used to create music-based games.  It provides a basic scene, several demo scenes, and resources needed to construct a simple game synced with music.  The kit is intended to work well in a decoupled workflow that separates engineering, design, and art into layers that can operate independently.

## Scenes

There are three scenes included with the game kit.

### Basic

A basic scene with the minimum required objects.  Use this scene as a starting point for new scenes.

### GameKit01

A very simple demo of the basic game elements: avatar, bullet, target, wall, and music.

### TTW

A perspective/isometric demo that plays Through The Wall while running a Zaxxon-style game. Uses **ttw-init.txt** as its starting script.

## Scene Setup

To make a new scene, open **Assets/Scenes/basic.unity** and then **Save As** a new scene name in the same folder.

A basic scene contains these objects (already set up in the Basic scene):

### MainCamera

The main camera object.  Its name must match exactly and cannot contain a space. Position it at 5,3,-5 with no rotation or scaling.

### DirectionalLight

The light source for the scene.

### EventSystem

The event system used for mouse, keyboard, and touch control.

### ESG

The Easy Street Games initialization object, containing these components:

- ESG Config: used to pass API keys, etc.
- ESG Set: used to pass game variables. Add new variables to the List field.
- ESG Script: used to specify the initial script to execute (see ESG Scripts)
- ESG Mouse Monitor: relays keyboard, mouse, and touch events

## Game Kit Objects

The game kit uses the following object types to create an interactive scene.  Some of the objects are simple prefabs and others use a combination of prefab and ESG script.  The GameKit01 and TTW scenes demonstrate how these objects can be used.

### Avatar

This represents the player in the game.  It responds to arrow keys to move and the space bar to fire bullets.  Alternatively, it can be set to use drag and tap control for mobile.

There is an Avatar prefab that defines the object's shape. It must have its **layer** set to **Ship**. It also contains an **ESG Attach Script** component that adds the custom ESG script logic when the scene starts.  The **Variable Key** field should be set to **avatarLogic**.

### Bullet

Bullets are fired by the avatar to destroy targets. If a bullet collides with a target, both objects are destroyed. If a bullet collides with a wall, only the bullet is destroyed.

There is a **BulletShape** prefab that defines the object's shape. It must have its **layer** set to **Bullet**.  When the scene starts, this prefab will automatically be used to create a new Bullet prefab containing the custom ESG script logic.

### Target

Targets are any object in the scene with a name starting with "Target". If the avatar collides with a target, only the avatar is destroyed.  If a bullet collides with a target, both objects are destroyed.

Target prefabs or in-scene objects can be created by starting their name with "Target" and ensuring they have a Collider component with the **Is Trigger** field checked. It must have its **layer** set to **Target**. See Target_Cube in Prefabs for a working example.

### Wall

Walls are any object in the scene with a name starting with "Wall".  If the avatar collides with a wall, only the avatar is destroyed.  If a bullet collides with a wall, only the bullet is destroyed. 

Wall prefabs or in-scene objects can be created by starting their name with "Wall" and ensuring they have a Collider component with the **Is Trigger** field checked. It must have its **layer** set to **Wall**. See Wall in Prefabs for a working example.

### Song

This represents the song being played.  It stays in sync with the avatar's movement so that the gameplay can be timed with the music.

Use the ESG object's ESG Set component to specify:

- songStart=0
  - where to start the song (in seconds)
- songId=TTW
  - TTW will load Assets/ESG/Audio/Runtime/Resources/TTW.mp3

### Timeline

This object triggers effects at specific times that are in sync with the music and avatar movement. The **sequence** prototype in ttw-protos.txt provides a working example that triggers camera changes synced to the song.

## Prefabs

### Avatar Prefab

Located at **Assets/ESG/Core/Runtime/Resources/Prefabs/Avatar.prefab**

A cube-based player representation with an elongated shape (scale 0.1 x 0.1 x 0.7). Components:

- **MeshFilter/MeshRenderer**: Unity cube mesh with custom material
- **BoxCollider**: Standard collision (non-trigger)
- **AnimList**: Animation state management
- **ESGAttachScript**: Attaches custom logic at runtime
  - variableKey: **avatarLogic** (references the avatar control script)
  - destroySelf: true (removes the attach script after initialization)

Must have its **layer** set to **Ship** (layer 8).

### BulletShape Prefab

Located at **Assets/ESG/Core/Runtime/Resources/Prefabs/BulletShape.prefab**

A small, flat, elongated cube (scale 0.1 x 0.05 x 0.4) used as the visual template for bullets. Components:

- **MeshFilter/MeshRenderer**: Unity cube mesh with custom material
- **BoxCollider**: Standard collision (non-trigger)

Initial Y position is -100 to keep it hidden below the scene. At runtime, ttw-prefabs.txt creates the actual **Bullet** prefab by cloning this shape and adding movement/collision logic.

### Wall Prefab

Located at **Assets/ESG/Core/Runtime/Resources/Prefabs/Wall.prefab**

A tall, thin wall shape (scale 1 x 3 x 0.1). Components:

- **MeshFilter/MeshRenderer**: Unity cube mesh with custom material
- **BoxCollider**: Set as **trigger** for collision detection

Must have its **layer** set to **Wall** (layer 11). Objects named with the "Wall" prefix will destroy bullets on contact and destroy the avatar on contact.

### Target_Cube Prefab

Located at **Assets/ESG/Core/Runtime/Resources/Prefabs/Target_Cube.prefab**

A unit cube (scale 1 x 1 x 1) used as a basic target. Components:

- **MeshFilter/MeshRenderer**: Unity cube mesh with custom material
- **BoxCollider**: Set as **trigger** for collision detection
- **AnimList**: Animation state management
- **ESGAttachScript**: Attaches custom logic at runtime
  - variableKey: **spinnerCW** (defined in ttw-prefabs.txt)
  - delay: 1 second before activation

Must have its **layer** set to **Target** (layer 10). Can be destroyed by bullets, which triggers the explosion effect.

## ESG Scripts

ESG scripts are stored in **Assets/ESG/Core/Runtime/Resources/ttw/** and provide the game logic for the Through The Wall demo. The scripts use a modular design where **ttw-init** loads all other modules.

### ttw-init.txt

The main initialization script that bootstraps the game. It:

- Sets the resource path for loading other scripts
- Defines game variables (useMouse, isoDur, collision prefixes)
- Calculates timing variables from songStart
- Loads all other script modules (camera, prefabs, protos, music, ui, game)

### ttw-prefabs.txt

Defines prefabs and logic for the Avatar and Bullet objects:

- **spinner** / **spinnerCCW**: Rotation variables for ESG Attach Script components
- Creates **avatarLogic** - the custom logic attached to the Avatar prefab:
  - Sets initial position and rigid body
  - Handles forward movement when the "go" signal is sent
  - Arrow key or swipe control for movement
  - Space bar or tap to fire bullets
  - Collision detection for walls and targets
- Creates the custom **Bullet** prefab from BulletShape with:
  - Forward movement and auto-destroy after 1.5 seconds
  - Explodes targets on collision
  - Fragments on wall collision

### ttw-game.txt

The main game loop script that:

- Sets up the background and curve library
- Defines the **restart** prototype that executes when the player dies:
  - Resets camera position
  - Destroys and respawns targets
  - Respawns the avatar and restarts music
- Contains the **init** logic that runs at game start:
  - Spawns the Avatar, timeline sequence, and music
  - Creates the height indicator and timer

### ttw-camera.txt

Handles camera behavior by attaching logic to the MainCamera:

- Enables the "I" key to toggle isometric view
- Defines a **cameraChange** state machine with these states:
  - **reset**: Default follow camera with no roll
  - **roll-left**: Rolls camera 30° left while following avatar
  - **roll-right**: Rolls camera 30° right while following avatar
  - **roll-center**: Returns camera roll to center
  - **iso**: Switches to orthographic projection with isometric angle
  - **persp**: Returns to perspective projection

### ttw-music.txt

Manages music playback:

- Creates the **music** prefab containing:
  - **allStem**: Loads the full song stem file
  - **audio**: The song player that starts at the configured songStart time
- Destroys music objects when stop.music signal is received
- Contains commented code for individual stem control (drums, synth, vox, bass, guitars)

### ttw-protos.txt

Defines reusable prototypes and prefabs:

- **sequence**: Timeline prototype that triggers camera changes at specific times synced to music (toggle iso at 27s and 35s, roll effects at 43s, 48s, 53s)
- **explode**: Explosion effect that fragments target objects
- **fragment**: Red cube that flies outward and shrinks when spawned
- **heightInd**: Cylinder below the avatar showing height position
- **TTW_TargetRocket**: Red cylinder that launches upward as a target

### ttw-ui.txt

Creates the game user interface:

- **GameUI** canvas with a side panel containing:
  - **timerText**: Displays the elapsed game time
- **clickMe**: A 3D start button that triggers the start.game signal when clicked

