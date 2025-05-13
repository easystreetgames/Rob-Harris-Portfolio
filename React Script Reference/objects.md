# Objects

## Ellipse / Circle
Creates an elliptical or circular shape with configurable properties.

### Parameters
- x (int): horizontal position of the ellipse's center
- y (int): vertical position of the ellipse's center
- width (int): width of the ellipse in pixels (diameter for circle)
- height (int): height of the ellipse in pixels (set equal to width for perfect circle)
- color (string): fill color of the ellipse
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor
- strokeWidth (int): width of the outline in pixels
- strokeColor (string): color of the outline

### Example
```
Ellipse(x=200 y=150 width=300 height=200 color=purple alpha=0.7)
```

```
Circle(x=100 y=100 width=80 height=80 color=red strokeWidth=3 strokeColor=black)
```

```
Ellipse(x=300 y=250 width=180 height=120 color=green rotation=0.25)
```

## Empty / Container
Creates an empty container object that can hold and manage multiple child objects.

### Parameters
- x (int): horizontal position of the container's center
- y (int): vertical position of the container's center
- id (string): unique identifier for the container
- width (int): width of the container in pixels
- height (int): height of the container in pixels
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- visible (boolean): whether the container and its children are visible

### Child Management
- Children added to the container will inherit the container's position, rotation, and scale
- Children can be added using the container as a parent in their definition
- When a child is removed from the container, its position is adjusted relative to the world

### Example
```
Empty(id='menu' x=400 y=300
  Text(x=0 y=-50 text='Main Menu' size=40 color=white)
  Text(x=0 y=0 text='Play Game' size=30 color=white)
  Text(x=0 y=50 text='Options' size=30 color=white)
)
```

```
Container(id='card' x=200 y=150
  Rectangle(width=120 height=160 color=white)
  Image(y=-40 src='card-icon.png' width=60 height=60)
  Text(y=40 text='Power Card' color=black)
)
```

## Image / Img
Creates an image object that displays images loaded from a specified source.

### Parameters
- x (int): horizontal position of the image's center
- y (int): vertical position of the image's center
- src / source (string): path or key to the image to display (default: "esg-icon")
- width (int): width of the image in pixels (uses natural width if not specified)
- height (int): height of the image in pixels (uses natural height if not specified)
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor
- color (string): tint color for the image
- strokeWidth (int): width of the border in pixels
- strokeColor (string): color of the border

### Example
```
Image(x=200 y=150 src='assets/player.png')
```

```
Image(x=100 y=300 src='logo.png' width=128 height=64 alpha=0.8)
```

```
Image(x=400 y=200 src='background.jpg' rotation=0.5 scale.x=1.5 scale.y=1.5)
```

## Rectangle / Rect
Creates a rectangular shape with configurable properties.

### Parameters
- x (int): horizontal position of the rectangle's center
- y (int): vertical position of the rectangle's center
- width (int): width of the rectangle in pixels
- height (int): height of the rectangle in pixels
- color (string): fill color of the rectangle
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor
- strokeWidth (int): width of the outline in pixels
- strokeColor (string): color of the outline

### Example
```
Rectangle(x=100 y=150 width=200 height=100 color=blue alpha=0.8)
```

```
Rect(x=50 y=50 width=80 height=30 color=red strokeWidth=2 strokeColor=black)
```

```
Rect(x=300 y=200 width=150 height=150 color=green rotation=0.5)
```

## Sprite
Creates a sprite object that can display frames from a sprite sheet or animated sequence.

### Parameters
- x (int): horizontal position of the sprite's center
- y (int): vertical position of the sprite's center
- src (string): path or key to the sprite sheet image (default: "esg-icon")
- width (int): width of each frame in pixels (uses natural width/columns if not specified)
- height (int): height of each frame in pixels (uses natural height/rows if not specified)
- frame / index / idx (int): current frame to display (default: 0)
- columns / cols (int): number of columns in the sprite sheet (default: 1)
- rows (int): number of rows in the sprite sheet (default: 1)
- alpha (float): transparency value (0-1, where 0 is invisible and 1 is fully opaque)
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor

### Example
```
Sprite(x=200 y=150 src='character.png' columns=8 rows=4 frame=0)
```

```
Sprite(x=100 y=300 src='explosion.png' columns=5 rows=1 width=64 height=64)
```

```
Sprite(x=400 y=200 src='coins.png' columns=6 rows=1 frame=2 scale.x=2 scale.y=2)
```

## Text
Creates a text object with customizable content, appearance, and background options.

### Parameters
- x (int): horizontal position of the text
- y (int): vertical position of the text
- text / txt / msg / content (string): the text content to display
- size / sz (int): font size in pixels (default: 30)
- font (string): font family name (default: "Arial")
- color (string): text color
- align (string): text alignment ("left", "center", "right") (default: "center")
- baseline / base (string): vertical alignment ("top", "middle", "bottom") (default: "middle")
- bgColor / bg (string): background color
- bgPadding / bgpad (int): padding around the text (default: 5)
- bgBorderRadius / bgradius / bgrad (int): corner radius for the background (default: 5)
- bgImage / bgImg / bgSrc (string): path to background image
- lineHeight / lh (float): line spacing multiplier (default: 1.2)
- lineLimit / limit (int): maximum number of lines to display (default: 15)
- charPerLine / charLimit / wrap (int): maximum characters per line for word wrapping
- strokeWidth (int): width of the text outline
- strokeColor (string): color of the text outline
- alpha (float): transparency value (0-1)
- rotation (float): rotation in radians
- scale.x (float): horizontal scaling factor
- scale.y (float): vertical scaling factor

### Example
```
Text(x=200 y=150 text='Hello World' size=40 color=black)
```

```
Text(x=100 y=300 text='Welcome to the game\nPress SPACE to start' align=left bgColor=blue color=white bgPadding=10)
```

```
Text(x=400 y=100 text='Game Over' size=60 color=red font='Impact' bgColor=#222222 bgBorderRadius=15)
```