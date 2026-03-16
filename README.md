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

- `!var` — get a variable's value
- `@list[0]` — get a list item by index
- `$var` — get the length of a list or string (returns length - 1, index-friendly)
- Line numbers are optional, but required for `goto` and `choice` jumps

```
setupGUI 400 400 MyWindow
var int x 200
var int y 200

20
choice !GUIshouldclose = 0 -> 10
    clearBkg black
    updateGUI
    draw circle !x !y 25 green
    key left held -> 30
        change x sub 3
    30
    key right held -> 31
        change x add 3
    31
    endGUI
    goto 20
10
closeGUI
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
| `random min max` | Set to random integer between min and max |

### Control Flow
| Syntax | Description |
|---|---|
| `goto N` | Jump to line number N |
| `goto` | Skip the next line |
| `choice !a = !b -> N` | Jump to N if condition is **false** |
| `choice !a < !b -> N` | Jump to N if a is not less than b |
| `choice !a > !b -> N` | Jump to N if a is not greater than b |

> `choice` jumps when the condition is **false** — use it as "if NOT this, skip to N"

### Functions
```
define func myFunc
    show hello
    change x add 1
endf

call myFunc
```
All variables are global — no arguments or return values needed.

### Keyboard Input
| Syntax | Description |
|---|---|
| `key a held -> N` | Jump to N if key is NOT held |
| `key space notheld -> N` | Jump to N if key IS held |

Supported keys: `a-z`, `0-9`, `up`, `down`, `left`, `right`, `space`, `enter`, `escape`, `backspace`, `tab`, `shift`, `ctrl`, `alt`, `f1-f12`

### Graphics
| Syntax | Description |
|---|---|
| `setupGUI width height title` | Open a window |
| `updateGUI` | Begin a new frame |
| `endGUI` | End the frame |
| `closeGUI` | Close the window |
| `clearBkg color` | Fill background |
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

### Colors
`black`, `white`, `blue`, `green`, `gray`, `brown`, `purple`, `darkgreen`, `darkgray`, `darkbrown`, `darkpurple`

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

### Bouncing Circle
```
setupGUI 400 400 Bounce
var int x 200
var int y 200
var int dx 3
var int dy 2

20
choice !GUIshouldclose = 0 -> 10
    clearBkg black
    updateGUI
    draw circle !x !y 20 green
    endGUI
    change x add !dx
    change y add !dy
    goto 20
10
closeGUI
```

### Functions
```
var int score 0

define func addScore
    change score add 10
    show !score
endf

call addScore
call addScore
call addScore
```
