# Objects

A script is a collection of display objects. When the script is parsed, it creates those display objects.  Each display object can have one or more [anims](./anims.md) attached to it.  Anims are updated once per frame to affect their display object or perform some other action.

There are several types of display objects:

- Shape objects
- Image objects
- Text objects
- Container objects
- State object

Special notation is used to find an existing object in a scene or create an object from a prototype definition:

- ?(object ID)
- $(prototype ID)

There is also one special command that is not an object:

- Define command

## Shape Objects

All shapes share basic parameters to configure the shape.

### Shared Parameters

- id (string): the object's ID
- x (int): horizontal position of the object's center
- y (int): vertical position of the object's center
- width / w (int): width of the object in pixels
- height / h (int): height of the object in pixels
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- visible / vis (boolean): visibility
- strokeWidth (int): width of the outline in pixels
- strokeColor / stroke (string): color of the outline
- state (string): object ID of a state contained by this object (see [State Machine Tutorial](./state-machines.md) for details)

### Rectangle / Rect

Creates a rectangular shape with configurable properties.

#### Parameters for Rectangle

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- color (string): fill color of the rectangle
- rotation (float): rotation in degrees

#### Example: Rectangle

```script
Rectangle(x=100 y=150 width=200 height=100 color=blue alpha=0.8)
```

```script
Rect(x=50 y=50 width=80 height=30 color=red strokeWidth=2 strokeColor=black)
```

```script
Rect(x=300 y=200 width=150 height=150 color=green rotation=50)
```

### Square

Creates a square shape with configurable properties.

#### Parameters for Square

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- color (string): fill color of the rectangle
- rotation (float): rotation in degrees
- size (int): width and height of the square

#### Example: Square

```script
Square(x=100 y=150 size=100 color=#223322)
```

#### Square Dimensions

The shape does not have to stay square; height and width can be modified after creation.

### Ellipse / Oval

Creates an elliptical shape with configurable properties.

#### Parameters for Ellipse

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- color (string): fill color of the ellipse
- rotation (float): rotation in degrees

#### Example: Ellipse

```script
Ellipse(x=200 y=150 width=300 height=200 color=purple alpha=0.7)
```

```script
Circle(x=100 y=100 width=80 height=80 color=red strokeWidth=3 strokeColor=black)
```

```script
Ellipse(x=300 y=250 width=180 height=120 color=green rotation=30)
```

### Circle

Creates a circle shape with configurable properties.

#### Parameters for Circle

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- color (string): fill color of the rectangle
- rotation (float): rotation in degrees
- radius (int): width x 2 and height x 2 of the circle

#### Example: Circle

```script
Circle(x=150 y=100 r=50 color=#223322)
```

#### Circle Dimensions

The shape does not have to stay round; height and width can be modified after creation.

## Text Objects

### Text

Creates a text object with customizable content, appearance, and background options.

#### Parameters for Text

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- color (string): text color
- rotation (float): rotation in degrees
- text / txt / msg / content (string): the text content to display
- size / sz (int): font size in pixels (default: 30)
- font (string): font family name (default: "Arial")
- align (string): text alignment ("left", "center", "right") (default: "center")
- baseline / base (string): vertical alignment ("top", "middle", "bottom") (default: "middle")
- bgColor / bg (string): background color
- bgPadding / bgpad (int): padding around the text (default: 5)
- bgBorderRadius / bgradius / bgrad (int): corner radius for the background (default: 5)
- bgImage / bgImg / bgSrc (string): path to background image
- lineHeight / lh (float): line spacing multiplier (default: 1.2)
- lineLimit / limit (int): maximum number of lines to display (default: 15)
- charPerLine / charLimit / wrap (int): maximum characters per line for word wrapping

The **text** property has a special value that can be used to show the ID of an object: **%self%**.  When used to set the text property, it will be replaced by the object's ID.

#### Example: Text

```script
Text('Hello World')
```

```script
Text(id=Hello x=-200 y=150 text=%self% size=40 color=black)
```

```script
Text(x=100 y=300 text='Welcome to the game\nPress SPACE to start' align=left bgColor=blue color=white bgPadding=10)
```

```script
Text(x=400 y=100 text='Game Over' size=60 color=red font='Impact' bgColor=#222222 bgBorderRadius=15)
```

## Image Objects

### Image / Img

Creates an image object that displays images loaded from a specified source.

#### Parameters for Image

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- rotation (float): rotation in degrees
- src / source (string): key of the image to display (default: "esg-icon")

#### Natural Dimensions

- natural width and height of the image are used if not specified

#### Example: Image

```script
Image(x=200 y=150 src='player.png')
```

```script
Image(x=100 y=300 src='logo.png' width=128 height=64 alpha=0.8)
```

```script
Image(x=400 y=200 src='background.jpg' rotation=180)
```

### Sprite

Creates a sprite object that can display frames from a sprite sheet or animated sequence.

#### Parameters for Sprite

- id, x, y, width, height, alpha, etc (basic shape parameters) 
- rotation (float): rotation in degrees
- src (string): key of the sprite sheet image (default: "esg-icon")
- frame / index / idx (int): current frame to display (default: 0)
- columns / cols (int): number of columns in the sprite sheet (default: 1)
- rows (int): number of rows in the sprite sheet (default: 1)

#### Example: Sprite

```script
Sprite(x=200 y=150 src='character.png' columns=8 rows=4 frame=0)
```

```script
Sprite(x=100 y=300 src='explosion.png' columns=5 rows=1 width=64 height=64)
```

```script
Sprite(x=400 y=200 src='coins.png' columns=6 rows=1 frame=2)
```

## Container Objects

### Container / Cont / Empty / _

Creates an empty container object that can hold and manage multiple child objects.

#### Parameters for Container

- id, x, y, width, height, alpha, etc (basic shape parameters) 

#### Container Children

- Children added to the container will inherit the container's position, rotation, and scale

#### Example: Container

```script
Empty(id='menu' x=400 y=300
  Text(x=0 y=-50 text='Main Menu' size=40 color=white)
  Text(x=0 y=0 text='Play Game' size=30 color=white)
  Text(x=0 y=50 text='Options' size=30 color=white)
)
```

```script
Container(id='card' x=200 y=150
  Rectangle(width=120 height=160 color=white)
  Image(y=-40 src='card-icon.png' width=60 height=60)
  Text(y=40 text='Power Card' color=black)
)
```

## State Objects

The State object is a special type of object used to create state machine and behavior tree constructs.  State objects are added to other types of objects to create a state machine construct.  See the [State Machine Tutorial](./state-machines.md) for more details.

## Special Notation

### ?(object ID)

An existing object in a scene can be referenced by prefixing the object's ID with a question mark.  This is helpful when adding a secondary script to a scene that will modify an existing object.

#### Example: ?

```script
?player(x=0 y=100)
```

### $(prototype ID)

Objects defined by the Define command (see next section) are called prototypes.  Once a prototype has been defined, copies can be created by using the dollar sign notation.

See the [Prototype tutorial](./prototypes.md) for more details.

#### Example: $

```script
$toast(text='hello world' x=0 y=-50)
```

## Commands

### Define

Variables and prototypes are defined with the Define command.

#### Variables

Key/value pairs can be defined for future use in a script.

```script
Define(
  buttonColor=#223322
  textColor=white
)

Text(text='Click Me' color=$buttonColor bg=$buttonColor)
```

#### Prototypes

Objects can be defined for future creation.  

See the [Prototype tutorial](./prototypes.md) for more details.

```script
Define(
  Text(id=toast size=30 color=#aaccaa x=50)
)

$toast(text='hello' y=100)
$toast(text='goodbye' y=200)
```

#### Prototype Definitions

Prototype definitions are not display objects.  They will not appear in the scene until a copy of a prototype is made.
