

Next: [Fringe Bitmaps](Fringe-Bitmaps.html), Previous: [Fringe Indicators](Fringe-Indicators.html), Up: [Fringes](Fringes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.13.3 Fringe Cursors

When a line is exactly as wide as the window, Emacs displays the cursor in the right fringe instead of using two lines. Different bitmaps are used to represent the cursor in the fringe depending on the current buffer’s cursor type.

*   User Option: **overflow-newline-into-fringe**

    If this is non-`nil`, lines exactly as wide as the window (not counting the final newline character) are not continued. Instead, when point is at the end of the line, the cursor appears in the right fringe.

<!---->

*   Variable: **fringe-cursor-alist**

    This variable specifies the mapping from logical cursor type to the actual fringe bitmaps displayed in the right fringe. The value is an alist where each element has the form `(cursor-type . bitmap)`, which means to use the fringe bitmap `bitmap` to display cursors of type `cursor-type`.

    Each `cursor-type` should be one of `box`, `hollow`, `bar`, `hbar`, or `hollow-small`. The first four have the same meanings as in the `cursor-type` frame parameter (see [Cursor Parameters](Cursor-Parameters.html)). The `hollow-small` type is used instead of `hollow` when the normal `hollow-rectangle` bitmap is too tall to fit on a specific display line.

    Each `bitmap` should be a symbol specifying the fringe bitmap to be displayed for that logical cursor type. See [Fringe Bitmaps](Fringe-Bitmaps.html).

    When `fringe-cursor-alist` has a buffer-local value, and there is no bitmap defined for a cursor type, the corresponding value from the default value of `fringes-indicator-alist` is used.
