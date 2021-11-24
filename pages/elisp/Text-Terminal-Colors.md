

### 29.23 Text Terminal Colors

Text terminals usually support only a small number of colors, and the computer uses small integers to select colors on the terminal. This means that the computer cannot reliably tell what the selected color looks like; instead, you have to inform your application which small integers correspond to which colors. However, Emacs does know the standard set of colors and will try to use them automatically.

The functions described in this section control how terminal colors are used by Emacs.

Several of these functions use or return *rgb values*, described in [Color Names](Color-Names.html).

These functions accept a display (either a frame or the name of a terminal) as an optional argument. We hope in the future to make Emacs support different colors on different text terminals; then this argument will specify which terminal to operate on (the default being the selected frame’s terminal; see [Input Focus](Input-Focus.html)). At present, though, the `frame` argument has no effect.

### Function: **tty-color-define** *name number \&optional rgb frame*

This function associates the color name `name` with color number `number` on the terminal.

The optional argument `rgb`, if specified, is an rgb value, a list of three numbers that specify what the color actually looks like. If you do not specify `rgb`, then this color cannot be used by `tty-color-approximate` to approximate other colors, because Emacs will not know what it looks like.

### Function: **tty-color-clear** *\&optional frame*

This function clears the table of defined colors for a text terminal.

### Function: **tty-color-alist** *\&optional frame*

This function returns an alist recording the known colors supported by a text terminal.

Each element has the form `(name number . rgb)` or `(name number)`. Here, `name` is the color name, `number` is the number used to specify it to the terminal. If present, `rgb` is a list of three color values (for red, green, and blue) that says what the color actually looks like.

### Function: **tty-color-approximate** *rgb \&optional frame*

This function finds the closest color, among the known colors supported for `display`, to that described by the rgb value `rgb` (a list of color values). The return value is an element of `tty-color-alist`.

### Function: **tty-color-translate** *color \&optional frame*

This function finds the closest color to `color` among the known colors supported for `display` and returns its index (an integer). If the name `color` is not defined, the value is `nil`.
