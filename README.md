# HGL — Hyperpower Graphics Language
A simple, QBasic-inspired graphics programming language that runs on Linux. Write `.hgl` files, run them with the `hgl` executable — no Python required.

---

## Installation
Download the `hgl` binary from the latest release and make it executable:
```bash
chmod +x hgl
```

Optionally add it to your PATH so you can run it from anywhere:
```bash
sudo mv hgl /usr/local/bin/
```

Then run any `.hgl` file:
```bash
hgl myprogram.hgl
```

---

## Language Overview
HGL is line-based — one instruction per line. Variables are referenced with `!` and line numbers (optional) are used for jumps and conditionals.

```
setupGUI 400 400 MyWindow
var int x 200

20
choice !GUIshouldclose = 0 -> 10
    clearBkg black
    updateGUI
    draw circle !x 200 30 green
    key left held -> 30
        change !x sub 2
    30
    key right held -> 31
        change !x add 2
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
| `var int name value` | Declare an integer variable |
| `var float name value` | Declare a float variable |
| `var char name value` | Declare a string variable |
| `show !var` | Print a variable or value to the terminal |

### Math & Operations
| Syntax | Description |
|---|---|
| `change !var add value` | Add to a variable |
| `change !var sub value` | Subtract from a variable |
| `change !var mul value` | Multiply a variable |
| `change !var div value` | Divide a variable |
| `change !var random min max` | Set variable to a random int between min and max |

### Control Flow
| Syntax | Description |
|---|---|
| `goto N` | Jump to line number N |
| `goto` | Skip the next line |
| `choice !a = !b -> N` | Jump to N if condition is FALSE |
| `choice !a < !b -> N` | Jump to N if a is not less than b |
| `choice !a > !b -> N` | Jump to N if a is not greater than b |

> **Note:** `choice` jumps when the condition is **false**, so use it as "if not X, skip to N".

### Keyboard Input
| Syntax | Description |
|---|---|
| `key a held -> N` | Jump to N if key is NOT held down |
| `key space pressed -> N` | Jump to N if key was NOT just pressed |

Supported keys: `a-z`, `0-9`, `up`, `down`, `left`, `right`, `space`, `enter`, `escape`, `backspace`, `tab`, `shift`, `ctrl`, `alt`, `f1-f12`

### Graphics
| Syntax | Description |
|---|---|
| `setupGUI width height title` | Open a window |
| `updateGUI` | Begin a new frame (call at start of draw loop) |
| `endGUI` | End the frame and present it |
| `closeGUI` | Close the window |
| `clearBkg color` | Fill background with a color |
| `draw circle x y radius color` | Draw a circle |
| `draw rect x y width height color` | Draw a rectangle |
| `draw line x1 y1 x2 y2 color` | Draw a line |

### Reserved Variables
| Variable | Description |
|---|---|
| `!GUIshouldclose` | Set to `1` when the window's X button is clicked |

### Colors
`black`, `white`, `blue`, `green`, `gray`, `brown`, `purple`, `darkgreen`, `darkgray`, `darkbrown`, `darkpurple`

---

## Line Numbers
Line numbers are optional but required for `goto` and `choice` jumps. Only label the lines you need to jump to.

```
10
goto 10
```

---

## Example — Moving Circle
```
setupGUI 400 400 MyWindow
var int x 200

20
choice !GUIshouldclose = 0 -> 10
    clearBkg black
    updateGUI
    draw circle !x 200 30 green
    key left held -> 30
        change !x sub 2
    30
    key right held -> 31
        change !x add 2
    31
    endGUI
    goto 20
10
closeGUI
```
