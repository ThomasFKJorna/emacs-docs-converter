

Next: [Fringe Cursors](Fringe-Cursors.html), Previous: [Fringe Size/Pos](Fringe-Size_002fPos.html), Up: [Fringes](Fringes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.13.2 Fringe Indicators

*Fringe indicators* are tiny icons displayed in the window fringe to indicate truncated or continued lines, buffer boundaries, etc.

*   User Option: **indicate-empty-lines**

    When this is non-`nil`, Emacs displays a special glyph in the fringe of each empty line at the end of the buffer, on graphical displays. See [Fringes](Fringes.html). This variable is automatically buffer-local in every buffer.

<!---->

*   User Option: **indicate-buffer-boundaries**

    This buffer-local variable controls how the buffer boundaries and window scrolling are indicated in the window fringes.

    Emacs can indicate the buffer boundaries—that is, the first and last line in the buffer—with angle icons when they appear on the screen. In addition, Emacs can display an up-arrow in the fringe to show that there is text above the screen, and a down-arrow to show there is text below the screen.

    There are three kinds of basic values:

    *   `nil`

        Don’t display any of these fringe icons.

    *   `left`

        Display the angle icons and arrows in the left fringe.

    *   `right`

        Display the angle icons and arrows in the right fringe.

    *   any non-alist

        Display the angle icons in the left fringe and don’t display the arrows.

    Otherwise the value should be an alist that specifies which fringe indicators to display and where. Each element of the alist should have the form `(indicator . position)`. Here, `indicator` is one of `top`, `bottom`, `up`, `down`, and `t` (which covers all the icons not yet specified), while `position` is one of `left`, `right` and `nil`.

    For example, `((top . left) (t . right))` places the top angle bitmap in left fringe, and the bottom angle bitmap as well as both arrow bitmaps in right fringe. To show the angle bitmaps in the left fringe, and no arrow bitmaps, use `((top . left) (bottom . left))`.

<!---->

*   Variable: **fringe-indicator-alist**

    This buffer-local variable specifies the mapping from logical fringe indicators to the actual bitmaps displayed in the window fringes. The value is an alist of elements `(indicator . bitmaps)`, where `indicator` specifies a logical indicator type and `bitmaps` specifies the fringe bitmaps to use for that indicator.

    Each `indicator` should be one of the following symbols:

    *   `truncation`, `continuation`.

        Used for truncation and continuation lines.

    *   `up`, `down`, `top`, `bottom`, `top-bottom`

        Used when `indicate-buffer-boundaries` is non-`nil`: `up` and `down` indicate a buffer boundary lying above or below the window edge; `top` and `bottom` indicate the topmost and bottommost buffer text line; and `top-bottom` indicates where there is just one line of text in the buffer.

    *   `empty-line`

        Used to indicate empty lines after the buffer end when `indicate-empty-lines` is non-`nil`.

    *   `overlay-arrow`

        Used for overlay arrows (see [Overlay Arrow](Overlay-Arrow.html)).

    Each `bitmaps` value may be a list of symbols `(left right [left1 right1])`. The `left` and `right` symbols specify the bitmaps shown in the left and/or right fringe, for the specific indicator. `left1` and `right1` are specific to the `bottom` and `top-bottom` indicators, and are used to indicate that the last text line has no final newline. Alternatively, `bitmaps` may be a single symbol which is used in both left and right fringes.

    See [Fringe Bitmaps](Fringe-Bitmaps.html), for a list of standard bitmap symbols and how to define your own. In addition, `nil` represents the empty bitmap (i.e., an indicator that is not shown).

    When `fringe-indicator-alist` has a buffer-local value, and there is no bitmap defined for a logical indicator, or the bitmap is `t`, the corresponding value from the default value of `fringe-indicator-alist` is used.

Next: [Fringe Cursors](Fringe-Cursors.html), Previous: [Fringe Size/Pos](Fringe-Size_002fPos.html), Up: [Fringes](Fringes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
