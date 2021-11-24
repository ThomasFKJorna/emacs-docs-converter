

#### 29.4.3.9 Cursor Parameters

This frame parameter controls the way the cursor looks.

`cursor-type`

How to display the cursor. Legitimate values are:

*   `box`

    Display a filled box. (This is the default.)

*   `hollow`

    Display a hollow box.

*   `nil`

    Don’t display a cursor.

*   `bar`

    Display a vertical bar between characters.

*   `(bar . width)`

    Display a vertical bar `width` pixels wide between characters.

*   `hbar`

    Display a horizontal bar.

*   `(hbar . height)`

    Display a horizontal bar `height` pixels high.

The `cursor-type` frame parameter may be overridden by the variables `cursor-type` and `cursor-in-non-selected-windows`:

### User Option: **cursor-type**

This buffer-local variable controls how the cursor looks in a selected window showing the buffer. If its value is `t`, that means to use the cursor specified by the `cursor-type` frame parameter. Otherwise, the value should be one of the cursor types listed above, and it overrides the `cursor-type` frame parameter.

### User Option: **cursor-in-non-selected-windows**

This buffer-local variable controls how the cursor looks in a window that is not selected. It supports the same values as the `cursor-type` frame parameter; also, `nil` means don’t display a cursor in nonselected windows, and `t` (the default) means use a standard modification of the usual cursor type (solid box becomes hollow box, and bar becomes a narrower bar).

### User Option: **x-stretch-cursor**

This variable controls the width of the block cursor displayed on extra-wide glyphs such as a tab or a stretch of white space. By default, the block cursor is only as wide as the font’s default character, and will not cover all of the width of the glyph under it if that glyph is extra-wide. A non-`nil` value of this variable means draw the block cursor as wide as the glyph under it. The default value is `nil`.

This variable has no effect on text-mode frames, since the text-mode cursor is drawn by the terminal out of Emacs’s control.

### User Option: **blink-cursor-alist**

This variable specifies how to blink the cursor. Each element has the form `(on-state . off-state)`. Whenever the cursor type equals `on-state` (comparing using `equal`), the corresponding `off-state` specifies what the cursor looks like when it blinks off. Both `on-state` and `off-state` should be suitable values for the `cursor-type` frame parameter.

There are various defaults for how to blink each type of cursor, if the type is not mentioned as an `on-state` here. Changes in this variable do not take effect immediately, only when you specify the `cursor-type` frame parameter.
