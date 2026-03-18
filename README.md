# HGL — Hyperpower Graphics Language
A simple, QBasic-inspired graphics programming language for Linux. Write `.hgl` files, run them with the `hgl` executable — nothing else required.

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

- `!var` — get a variable's value
- `@list[0]` — get a list item by index
- `@func{arg}` — get a function argument value
- `$var` — get the length of a list or string (returns length - 1, index-friendly)
- Line numbers are optional, but required for `goto` and `choice` jumps

```
gui setup 400 400 MyWindow
var int x 200
var int y 200

20
choice !GUIshouldclose = 0 -> 10
    gui clear niceblue
    gui update
    draw circle !x !y 25 green
    gui key left held -> 30
        change x sub 3
    30
    gui key right held -> 31
        change x add 3
    31
    gui end
    goto 20
10
gui close
```

---

## Commands

### Variables
| Syntax | Description |
|---|---|
| `var int name value` | Integer variable |
| `var double name value` | Float variable |
| `var char name value` | String variable |
| `var list name a b c ...` | List variable |
| `warp name int` | Convert variable to int |
| `warp name float` | Convert variable to float |
| `warp name char` | Convert variable to string |
| `del varName` | Delete a variable from memory |
| `show !var` | Print value to terminal |

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
All variables are global — function arguments are accessed with `@funcName{argName}` syntax.

### Lists
```
var list items apple banana cherry
var int i 0
show @items[0]          # prints: apple
show $items             # prints: 2 (last valid index)
edit items change 1 grape
edit items remove 0
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
| `gui key a held -> N` | Jump to N if key is NOT held |
| `gui key space notheld -> N` | Jump to N if key IS held |

### Drawing
| Syntax | Description |
|---|---|
| `draw circle x y radius color` | Draw a circle |
| `draw rect x y width height color` | Draw a rectangle |
| `draw line x1 y1 x2 y2 color` | Draw a line |
| `draw text string x y size color` | Draw text |

### Utility
| Syntax | Description |
|---|---|
| `wait seconds` | Pause execution |

### Reserved Variables
| Variable | Description |
|---|---|
| `!GUIshouldclose` | `1` when the window X button is clicked |

### Built-in Colors
`black`, `white`, `blue`, `green`, `gray`, `brown`, `purple`, `darkgreen`, `darkgray`, `darkbrown`, `darkpurple`, `niceblue`

### Custom Colors
```
gui color mycolor 324234423
gui clear mycolor
draw circle 200 200 50 mycolor
```

---

## Examples

### List Iteration
```
var list items apple banana cherry
var int i 0

10
choice !i < $items -> 20
    show @items[!i]
    change i add 1
    goto 10
20
```

### Functions with Arguments
```
define func greet <- name
    show @greet{name}
endf

call greet <- world
call greet <- HGL
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
choice !GUIshouldclose = 0 -> 1
gui update
gui clear niceblue
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
choice !ballX < 50 -> 24
choice !ballY > !plY -> 24
choice !ballY < !plYb -> 24
    change ballXv mul -1
24
gui end
goto 2
1
gui close
```
