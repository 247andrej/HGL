# HGL — Hyperpower Graphics Language
A simple, QBasic-inspired graphics programming language for Linux. Write `.hgl` files, run them with the `hgl` executable — no Python required.

---

## Installation
Download the `hgl` binary from the latest release and make it executable:
```bash
chmod +x hgl
sudo mv hgl /usr/local/bin/
```

Run any `.hgl` file:
```bash
hgl myprogram.hgl
```

---

## Language Overview
HGL is line-based — one instruction per line. No semicolons, no brackets, no noise.

**Variable access operators:**
- `!var` — get a variable's value
- `@list[0]` — get a list item by index
- `@func{arg}` — get a function argument value
- `@dict(key)` — get a dict value by key
- `$var` — get the length of a list or string (returns length - 1, index-friendly)
- `~!var` — get the ASCII value of a character
- `value<-i` — cast value to int
- `value<-f` — cast value to float
- `var<-w` — get width of a texture variable
- `var<-h` — get height of a texture variable
- `"string"` — inline string literal

Line numbers are optional but required for `goto` and `choice` jumps.

> **Note:** When comparing against literal numbers with `choice`, use `<-i` to cast them to int — e.g. `choice !GUIshouldclose = 0<-i -> 1`. Without it, the literal `0` is treated as a string and the comparison will always fail.

---

## Commands

### Variables
| Syntax | Description |
|---|---|
| `var int name value` | Integer variable |
| `var double name value` | Float variable |
| `var char name value` | String variable |
| `var list name a b c ...` | List variable |
| `var text name ./path.png` | Load a texture from file |
| `var utext name` | Unload a texture from memory |
| `warp name int` | Convert variable to int |
| `warp name float` | Convert variable to float |
| `warp name char` | Convert variable to string |
| `del varName` | Delete a variable from memory |
| `show !var` | Print value to terminal |

Add `*` at the end of a `var` declaration to force all values to resolve as variables:
```
var int w a<-w *
```

### Math & Operations
`change varName operation value`

| Operation | Description |
|---|---|
| `add` | Add |
| `sub` | Subtract |
| `mul` | Multiply |
| `div` | Divide |
| `sin !var` | Set to sin of a variable |
| `cos !var` | Set to cos of a variable |
| `sqrt` | Square root of current value |
| `random min max` | Set to random integer between min and max |

### Control Flow
| Syntax | Description |
|---|---|
| `goto N` | Jump to line number N |
| `goto` | Skip the next line |
| `choice !a = !b -> N` | Jump to line N if condition is **false** |
| `choice !a < !b -> N` | Jump to N if a is not less than b |
| `choice !a > !b -> N` | Jump to N if a is not greater than b |
| `choice !a = !b <- N` | Skip N lines if condition is **false** |

> `choice` jumps/skips when the condition is **false** — use it as "if NOT this, go to N"

### Functions
```
define func myFunc
    show hello
endf

define func myFuncWithArgs <- x y
    show @myFuncWithArgs{x}
    show @myFuncWithArgs{y}
endf

call myFunc
call myFuncWithArgs <- 10 20
```

### Lists
```
var list items apple banana cherry
show @items[0]
show $items
edit list items change 1 grape
edit list items remove 0
```

### File I/O
```
file txt create notes
file txt write notes
hello world
endfw

file txt read notes -> myVar
show !myVar

file json create data
file json write data
{
    "score": 0,
    "name": "player"
}
endfw

file json read data -> saveData
show @saveData(score)
edit dict saveData change score 999<-i
```

### GUI
| Syntax | Description |
|---|---|
| `gui setup width height title` | Open a window |
| `gui update` | Begin a new frame |
| `gui end` | End the frame |
| `gui close` | Close the window |
| `gui clear color` | Fill background |
| `gui color name hexvalue` | Define a custom color from a hex integer |
| `gui goto x y` | Move the window to a screen position |
| `gui resize w h` | Resize the window |
| `gui ek key` | Change the exit key (default: escape) |
| `gui change tex width value` | Set texture width |
| `gui change tex height value` | Set texture height |
| `gui key a held -> N` | Jump to N if key is NOT held |
| `gui key space notheld -> N` | Jump to N if key IS held |
| `gui mouse left held -> N` | Jump to N if mouse button is NOT held |
| `gui check recs x1 y1 w1 h1 x2 y2 w2 h2 -> N` | Jump to N if rectangles are NOT colliding |

### Drawing
| Syntax | Description |
|---|---|
| `draw circle x y radius color` | Draw a circle |
| `draw rect x y width height color` | Draw a rectangle |
| `draw line x1 y1 x2 y2 color` | Draw a line |
| `draw text string x y size color` | Draw text |
| `draw texture !tex x y` | Draw a texture |
| `draw texture !tex x y color` | Draw a texture with tint |
| `draw textureEx !tex x y rotation scale` | Draw a texture with rotation and scale |
| `draw textureEx !tex x y rotation scale color` | Draw a texture with rotation, scale, and tint |

### Utility
| Syntax | Description |
|---|---|
| `wait seconds` | Pause execution |

### Reserved Variables
| Variable | Description |
|---|---|
| `!GUIshouldclose` | `1` when the window X button is clicked |
| `!mouseX` | Current mouse X position |
| `!mouseY` | Current mouse Y position |

### Built-in Colors
`black`, `white`, `blue`, `green`, `gray`, `brown`, `purple`, `darkgreen`, `darkgray`, `darkbrown`, `darkpurple`, `niceblue`

---

## Examples

### Texture with proportional scaling
```
gui setup 400 400 demo

var text img ./sprite.png

var int w img<-w *
var int h img<-h *

change w div 2
change h div 2

gui change img width !w
gui change img height !h

2
choice !GUIshouldclose = 0<-i -> 1
gui update
gui clear black
draw texture !img 0 0
gui end
goto 2
1
gui close
```

### String comparison
```
var char a hello
var char b hello

var int i 0
var int areTheSame 0

choice $a < $b -> 1
choice $a > $b -> 1

2
choice !i < $a -> 1
    choice ~@a[!i] = ~@b[!i] -> 1
        change i add 1
        choice !i = $a -> 2
            show same
            goto 3
        goto 2
1
show not the same
3
```

### Pong
```
gui setup 600 400 pong
var int plX 20
var int plY 100
var int plVel 6
var int plYb 180
var int ballX 100
var int ballY 100
var int ballXv 4
var int ballYv 4

2
choice !GUIshouldclose = 0<-i -> 1
gui update
gui clear black
gui key w held -> 10
    change plY sub !plVel
    change plYb sub !plVel
10
gui key s held -> 11
    change plY add !plVel
    change plYb add !plVel
11
draw rect !plX !plY 20 80 white
draw circle !ballX !ballY 10 white
change ballX add !ballXv
change ballY add !ballYv
choice !ballY > 390 -> 20
    change ballYv mul -1
20
choice !ballY < 10 -> 21
    change ballYv mul -1
21
choice !ballX > 590 -> 22
    change ballXv mul -1
22
choice !ballX < 0 -> 23
    goto 1
23
gui check recs !plX !plY 20 80 !ballX !ballY 20 20 -> 24
    change ballXv mul -1
24
gui end
goto 2
1
gui close
```
