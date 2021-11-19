

Next: [Drag Events](Drag-Events.html), Previous: [Mouse Events](Mouse-Events.html), Up: [Input Events](Input-Events.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 21.7.4 Click Events

When the user presses a mouse button and releases it at the same location, that generates a *click* event. All mouse click event share the same format:

```lisp
(event-type position click-count)
```

*   `event-type`

    This is a symbol that indicates which mouse button was used. It is one of the symbols `mouse-1`, `mouse-2`, …, where the buttons are numbered left to right.

    You can also use prefixes ‘`A-`’, ‘`C-`’, ‘`H-`’, ‘`M-`’, ‘`S-`’ and ‘`s-`’ for modifiers alt, control, hyper, meta, shift and super, just as you would with function keys.

    This symbol also serves as the event type of the event. Key bindings describe events by their types; thus, if there is a key binding for `mouse-1`, that binding would apply to all events whose `event-type` is `mouse-1`.

*   `position`

    This is a *mouse position list* specifying where the mouse click occurred; see below for details.

*   `click-count`

    This is the number of rapid repeated presses so far of the same mouse button. See [Repeat Events](Repeat-Events.html).

To access the contents of a mouse position list in the `position` slot of a click event, you should typically use the functions documented in [Accessing Mouse](Accessing-Mouse.html).

The explicit format of the list depends on where the click occurred. For clicks in the text area, mode line, header line, tab line, or in the fringe or marginal areas, the mouse position list has the form

```lisp
(window pos-or-area (x . y) timestamp
 object text-pos (col . row)
 image (dx . dy) (width . height))
```

The meanings of these list elements are as follows:

*   `window`

    The window in which the click occurred.

*   `pos-or-area`

    The buffer position of the character clicked on in the text area; or, if the click was outside the text area, the window area where it occurred. It is one of the symbols `mode-line`, `header-line`, `tab-line`, `vertical-line`, `left-margin`, `right-margin`, `left-fringe`, or `right-fringe`.

    In one special case, `pos-or-area` is a list containing a symbol (one of the symbols listed above) instead of just the symbol. This happens after the imaginary prefix keys for the event are registered by Emacs. See [Key Sequence Input](Key-Sequence-Input.html).

*   `x`, `y`

    The relative pixel coordinates of the click. For clicks in the text area of a window, the coordinate origin `(0 . 0)` is taken to be the top left corner of the text area. See [Window Sizes](Window-Sizes.html). For clicks in a mode line, header line or tab line, the coordinate origin is the top left corner of the window itself. For fringes, margins, and the vertical border, `x` does not have meaningful data. For fringes and margins, `y` is relative to the bottom edge of the header line. In all cases, the `x` and `y` coordinates increase rightward and downward respectively.

*   `timestamp`

    The time at which the event occurred, as an integer number of milliseconds since a system-dependent initial time.

*   `object`

    Either `nil`, which means the click occurred on buffer text, or a cons cell of the form (`string` . `string-pos`) if there is a string from a text property or an overlay at the click position.

    *   `string`

        The string which was clicked on, including any properties.

    *   `string-pos`

        The position in the string where the click occurred.

*   `text-pos`

    For clicks on a marginal area or on a fringe, this is the buffer position of the first visible character in the corresponding line in the window. For clicks on the mode line, the header line or the tab line, this is `nil`. For other events, it is the buffer position closest to the click.

*   `col`, `row`

    These are the actual column and row coordinate numbers of the glyph under the `x`, `y` position. If `x` lies beyond the last column of actual text on its line, `col` is reported by adding fictional extra columns that have the default character width. Row 0 is taken to be the header line if the window has one, or Row 1 if the window also has the tab line, or the topmost row of the text area otherwise. Column 0 is taken to be the leftmost column of the text area for clicks on a window text area, or the leftmost mode line or header line column for clicks there. For clicks on fringes or vertical borders, these have no meaningful data. For clicks on margins, `col` is measured from the left edge of the margin area and `row` is measured from the top of the margin area.

*   `image`

    If there is an image at the click location, this is the image object as returned by `find-image` (see [Defining Images](Defining-Images.html)); otherwise this is `nil`.

*   `dx`, `dy`

    These are the pixel coordinates of the click, relative to the top left corner of `object`, which is `(0 . 0)`. If `object` is `nil`, which stands for a buffer, the coordinates are relative to the top left corner of the character glyph clicked on.

*   `width`, `height`

    These are the pixel width and height of `object` or, if this is `nil`, those of the character glyph clicked on.

For clicks on a scroll bar, `position` has this form:

```lisp
(window area (portion . whole) timestamp part)
```

*   `window`

    The window whose scroll bar was clicked on.

*   `area`

    This is the symbol `vertical-scroll-bar`.

*   `portion`

    The number of pixels from the top of the scroll bar to the click position. On some toolkits, including GTK+, Emacs cannot extract this data, so the value is always `0`.

*   `whole`

    The total length, in pixels, of the scroll bar. On some toolkits, including GTK+, Emacs cannot extract this data, so the value is always `0`.

*   `timestamp`

    The time at which the event occurred, in milliseconds. On some toolkits, including GTK+, Emacs cannot extract this data, so the value is always `0`.

*   `part`

    The part of the scroll bar on which the click occurred. It is one of the symbols `handle` (the scroll bar handle), `above-handle` (the area above the handle), `below-handle` (the area below the handle), `up` (the up arrow at one end of the scroll bar), or `down` (the down arrow at one end of the scroll bar).

For clicks on the frame’s internal border (see [Frame Layout](Frame-Layout.html)), `position` has this form:

```lisp
 (frame part (X . Y) timestamp)
```

*   `frame`

    The frame whose internal border was clicked on.

*   `part`

    The part of the internal border which was clicked on. This can be one of the following:

    *   `nil`

        The frame does not have an internal border. This usually happens on text-mode frames. This can also happen on GUI frames with internal border if the frame doesn’t have its `drag-internal-border` parameter (see [Mouse Dragging Parameters](Mouse-Dragging-Parameters.html)) set to a non-`nil` value.

    *   *   `left-edge`
        *   `top-edge`
        *   `right-edge`
        *   `bottom-edge`

        The click was on the corresponding border at an offset of at least one canonical character from the border’s nearest corner.

    *   *   `top-left-corner`
        *   `top-right-corner`
        *   `bottom-right-corner`
        *   `bottom-left-corner`

        The click was on the corresponding corner of the internal border.

Next: [Drag Events](Drag-Events.html), Previous: [Mouse Events](Mouse-Events.html), Up: [Input Events](Input-Events.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
