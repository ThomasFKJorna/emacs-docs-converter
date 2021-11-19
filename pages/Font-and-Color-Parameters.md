

Previous: [Cursor Parameters](Cursor-Parameters.html), Up: [Window Frame Parameters](Window-Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 29.4.3.10 Font and Color Parameters

These frame parameters control the use of fonts and colors.

*   `font-backend`

    A list of symbols, specifying the *font backends* to use for drawing characters on the frame, in order of priority. In Emacs built without Cairo drawing on X, there are currently three potentially available font backends: `x` (the X core font driver), `xft` (the Xft font driver), and `xfthb` (the Xft font driver with HarfBuzz text shaping). If built with Cairo drawing, there are also three potentially available font backends on X: `x`, `ftcr` (the FreeType font driver on Cairo), and `ftcrhb` (the FreeType font driver on Cairo with HarfBuzz text shaping). When Emacs is built with HarfBuzz, the default font driver is `ftcrhb`, although use of the `ftcr` driver is still possible, but not recommended. On MS-Windows, there are currently three available font backends: `gdi` (the core MS-Windows font driver), `uniscribe` (font driver for OTF and TTF fonts with text shaping by the Uniscribe engine), and `harfbuzz` (font driver for OTF and TTF fonts with HarfBuzz text shaping) (see [Windows Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Windows-Fonts.html#Windows-Fonts) in The GNU Emacs Manual). The `harfbuzz` driver is similarly recommended. On other systems, there is only one available font backend, so it does not make sense to modify this frame parameter.

*   `background-mode`

    This parameter is either `dark` or `light`, according to whether the background color is a light one or a dark one.

*   `tty-color-mode`

    This parameter overrides the terminal’s color support as given by the system’s terminal capabilities database in that this parameter’s value specifies the color mode to use on a text terminal. The value can be either a symbol or a number. A number specifies the number of colors to use (and, indirectly, what commands to issue to produce each color). For example, `(tty-color-mode . 8)` specifies use of the ANSI escape sequences for 8 standard text colors. A value of -1 turns off color support.

    If the parameter’s value is a symbol, it specifies a number through the value of `tty-color-mode-alist`, and the associated number is used instead.

*   `screen-gamma`

    If this is a number, Emacs performs gamma correction which adjusts the brightness of all colors. The value should be the screen gamma of your display.

    Usual PC monitors have a screen gamma of 2.2, so color values in Emacs, and in X windows generally, are calibrated to display properly on a monitor with that gamma value. If you specify 2.2 for `screen-gamma`, that means no correction is needed. Other values request correction, designed to make the corrected colors appear on your screen the way they would have appeared without correction on an ordinary monitor with a gamma value of 2.2.

    If your monitor displays colors too light, you should specify a `screen-gamma` value smaller than 2.2. This requests correction that makes colors darker. A screen gamma value of 1.5 may give good results for LCD color displays.

*   `alpha`

    This parameter specifies the opacity of the frame, on graphical displays that support variable opacity. It should be an integer between 0 and 100, where 0 means completely transparent and 100 means completely opaque. It can also have a `nil` value, which tells Emacs not to set the frame opacity (leaving it to the window manager).

    To prevent the frame from disappearing completely from view, the variable `frame-alpha-lower-limit` defines a lower opacity limit. If the value of the frame parameter is less than the value of this variable, Emacs uses the latter. By default, `frame-alpha-lower-limit` is 20.

    The `alpha` frame parameter can also be a cons cell `(active . inactive)`, where `active` is the opacity of the frame when it is selected, and `inactive` is the opacity when it is not selected.

    Some window systems do not support the `alpha` parameter for child frames (see [Child Frames](Child-Frames.html)).

The following frame parameters are semi-obsolete in that they are automatically equivalent to particular face attributes of particular faces (see [Standard Faces](https://www.gnu.org/software/emacs/manual/html_node/emacs/Standard-Faces.html#Standard-Faces) in The Emacs Manual):

*   `font`

    The name of the font for displaying text in the frame. This is a string, either a valid font name for your system or the name of an Emacs fontset (see [Fontsets](Fontsets.html)). It is equivalent to the `font` attribute of the `default` face.

*   `foreground-color`

    The color to use for the image of a character. It is equivalent to the `:foreground` attribute of the `default` face.

*   `background-color`

    The color to use for the background of characters. It is equivalent to the `:background` attribute of the `default` face.

*   `mouse-color`

    The color for the mouse pointer. It is equivalent to the `:background` attribute of the `mouse` face.

*   `cursor-color`

    The color for the cursor that shows point. It is equivalent to the `:background` attribute of the `cursor` face.

*   `border-color`

    The color for the border of the frame. It is equivalent to the `:background` attribute of the `border` face.

*   `scroll-bar-foreground`

    If non-`nil`, the color for the foreground of scroll bars. It is equivalent to the `:foreground` attribute of the `scroll-bar` face.

*   `scroll-bar-background`

    If non-`nil`, the color for the background of scroll bars. It is equivalent to the `:background` attribute of the `scroll-bar` face.

Previous: [Cursor Parameters](Cursor-Parameters.html), Up: [Window Frame Parameters](Window-Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
