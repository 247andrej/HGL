# HGL - Hyperpower Graphics Language

A QBasic-inspired graphics programming language written in Python, powered by [Raylib](https://www.raylib.com/).

HGL is designed for simple 2D graphics programs — draw shapes, handle input, render textures, and build small games using a straightforward command-based syntax.

## Usage

Download the latest release from [Releases](../../releases) and run:

```bash
./hgl yourprogram.hgl
```

## Language Reference

### Variables

```
var int x 10
var double y 3.14
var char name Hello
var list items 1 2 3
var text sprite ./player.png
```

**Types:** `int`, `double`, `char`, `list`, `text` (texture), `utext` (unload texture)

### Variable Resolution

| Prefix/Suffix | Meaning | Example |
|---|---|---|
| `!` | Get variable value | `!x` |
| `@name[i]` | List index | `@items[0]` |
| `@name{key}` | Function argument | `@myFunc{arg1}` |
| `~` | Char to ASCII code | `~!letter` |
| `$` | Length - 1 | `$items` |
| `"text"` | String literal | `"hello"` |
| `<-f` | Cast to float | `!x<-f` |
| `<-i` | Cast to int | `!y<-i` |
| `<-w` | Texture width | `sprite<-w` |
| `<-h` | Texture height | `sprite<-h` |

### Output

```
show "Hello" !name
```

### Math & Operations

```
change x add 5
change x sub 1
change x mul 2
change x div 3
change x mod 2
change x random 1 10
change x sin !angle
change x cos !angle
change x sqrt
change x abs
change x pow 2
change x flr
change x cel
change x clamp 0 100
```

### Control Flow

```
choice !x = 10 -> 200
choice !name = "hello" -> 300
choice !x > 5 <- 3
```

Comparators: `=`, `<`, `>` (numeric only for `<` and `>`)

Arrow `->` jumps to a label. Arrow `<-` jumps by a relative offset.

### Labels & Goto

Lines starting with a number are labels:

```
100
show "this is label 100"
goto 100
```

### Functions

```
define func greet
show "Hello!"
endf

call greet
```

With arguments:

```
define func add <- a b
show @add{a} @add{b}

var int c @add{a} *
var int d @add{b} *

change c add !d
show !c
endf

call add <- 5 10
```

### Type Conversion

```
warp x int
warp y float
warp z char
```

### Lists

```
var list myList 1 2 3
edit list myList change 0 99
edit list myList remove 1
```

### Dictionaries

```
edit dict myDict change key "value"
edit dict myDict remove key
```

### File I/O

```
file txt create myfile
file txt write myfile
This content gets written to the file.
endfw
file txt read myfile -> contents
file json read data -> myData
```

Supported types: `txt`, `json`

### Directory Operations

```
dir create newfolder
dir delete oldfolder
dir list ./mydir -> files
```

### System Commands

```
sys [ls -la]
sys [echo !myVar] *
```

The `*` suffix resolves HGL variables inside the brackets.

### Wait

```
wait 1.5
```

---

## Graphics

### Window Setup

```
gui setup 800 600 MyGame
gui fps 60
gui ek escape
gui title "New Title"
gui goto 100 100
gui resize 1024 768
```

### Main Loop Pattern

```
100
gui update
gui clear black

draw rect 100 100 50 50 blue
draw circle 200 200 25 red
draw text "Score:" 10 10 20 white
draw line 0 0 800 600 purple
draw texture !mySprite 100 100
draw textureEx !mySprite 100 100 0.0 2.0 white

gui end
choice !GUIshouldclose = 0 -> 100

gui close
```

### Drawing Commands

| Command | Arguments |
|---|---|
| `draw rect` | x y width height color |
| `draw circle` | x y radius color |
| `draw line` | x1 y1 x2 y2 color |
| `draw text` | "string" x y size color |
| `draw texture` | texture x y [color] |
| `draw textureEx` | texture x y rotation scale [color] |

### Built-in Colors

`black`, `blue`, `white`, `green`, `gray`, `brown`, `purple`, `darkgreen`, `darkgray`, `darkbrown`, `darkpurple`

Custom colors:

```
gui color mycolor 4278190335
```

### Input

**Keyboard:**

```
gui key a held -> 200
gui key space notheld -> 300
```

**Mouse:**

```
gui mouse left held -> 200
gui mouse right notheld -> 300
```

**Built-in variables:** `mouseX`, `mouseY`, `GUIshouldclose`

### Collision Detection

```
gui check recs !x1 !y1 !w1 !h1 !x2 !y2 !w2 !h2 -> 500
```

### Texture Manipulation

```
gui change mySprite width 64
gui change mySprite height 64
```

---

## Built-in Variables

| Variable | Description |
|---|---|
| `GUIshouldclose` | `1` when window close is requested |
| `mouseX` | Current mouse X position |
| `mouseY` | Current mouse Y position |

## Example: Bouncing Box

```
var int x 100
var int y 100
var int dx 3
var int dy 2

gui setup 800 600 Bounce

100
gui update
gui clear black

change x add !dx
change y add !dy

choice !x > 750<-i <- 2
    change dx mul -1<-i
choice !x < 0<-i <- 2
    change dx mul -1<-i
choice !y > 550<-i <- 2
    change dy mul -1<-i
choice !y < 0<-i <- 2
    change dy mul -1<-i

draw rect !x !y 50 50 blue

gui end
choice !GUIshouldclose = 1<-i -> 100
gui close
```

## License

MIT
