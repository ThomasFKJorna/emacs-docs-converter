

Next: [Text Terminal Colors](Text-Terminal-Colors.html), Previous: [Drag and Drop](Drag-and-Drop.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.22 Color Names

A color name is text (usually in a string) that specifies a color. Symbolic names such as ‘`black`’, ‘`white`’, ‘`red`’, etc., are allowed; use `M-x list-colors-display` to see a list of defined names. You can also specify colors numerically in forms such as ‘`#rgb`’ and ‘`RGB:r/g/b`’, where `r` specifies the red level, `g` specifies the green level, and `b` specifies the blue level. You can use either one, two, three, or four hex digits for `r`; then you must use the same number of hex digits for all `g` and `b` as well, making either 3, 6, 9 or 12 hex digits in all. (See the documentation of the X Window System for more details about numerical RGB specification of colors.)

These functions provide a way to determine which color names are valid, and what they look like. In some cases, the value depends on the *selected frame*, as described below; see [Input Focus](Input-Focus.html), for the meaning of the term “selected frame”.

To read user input of color names with completion, use `read-color` (see [read-color](High_002dLevel-Completion.html)).

*   Function: **color-defined-p** *color \&optional frame*

    This function reports whether a color name is meaningful. It returns `t` if so; otherwise, `nil`. The argument `frame` says which frame’s display to ask about; if `frame` is omitted or `nil`, the selected frame is used.

    Note that this does not tell you whether the display you are using really supports that color. When using X, you can ask for any defined color on any kind of display, and you will get some result—typically, the closest it can do. To determine whether a frame can really display a certain color, use `color-supported-p` (see below).

    This function used to be called `x-color-defined-p`, and that name is still supported as an alias.

<!---->

*   Function: **defined-colors** *\&optional frame*

    This function returns a list of the color names that are defined and supported on frame `frame` (default, the selected frame). If `frame` does not support colors, the value is `nil`.

    This function used to be called `x-defined-colors`, and that name is still supported as an alias.

<!---->

*   Function: **color-supported-p** *color \&optional frame background-p*

    This returns `t` if `frame` can really display the color `color` (or at least something close to it). If `frame` is omitted or `nil`, the question applies to the selected frame.

    Some terminals support a different set of colors for foreground and background. If `background-p` is non-`nil`, that means you are asking whether `color` can be used as a background; otherwise you are asking whether it can be used as a foreground.

    The argument `color` must be a valid color name.

<!---->

*   Function: **color-gray-p** *color \&optional frame*

    This returns `t` if `color` is a shade of gray, as defined on `frame`’s display. If `frame` is omitted or `nil`, the question applies to the selected frame. If `color` is not a valid color name, this function returns `nil`.

<!---->

*   Function: **color-values** *color \&optional frame*

    This function returns a value that describes what `color` should ideally look like on `frame`. If `color` is defined, the value is a list of three integers, which give the amount of red, the amount of green, and the amount of blue. Each integer ranges in principle from 0 to 65535, but some displays may not use the full range. This three-element list is called the *rgb values* of the color.

    If `color` is not defined, the value is `nil`.

    ```lisp
    (color-values "black")
         ⇒ (0 0 0)
    (color-values "white")
         ⇒ (65280 65280 65280)
    (color-values "red")
         ⇒ (65280 0 0)
    (color-values "pink")
         ⇒ (65280 49152 51968)
    (color-values "hungry")
         ⇒ nil
    ```

    The color values are returned for `frame`’s display. If `frame` is omitted or `nil`, the information is returned for the selected frame’s display. If the frame cannot display colors, the value is `nil`.

    This function used to be called `x-color-values`, and that name is still supported as an alias.

Next: [Text Terminal Colors](Text-Terminal-Colors.html), Previous: [Drag and Drop](Drag-and-Drop.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
