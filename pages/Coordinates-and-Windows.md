<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html), Previous: [Horizontal Scrolling](Horizontal-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.24 Coordinates and Windows

This section describes functions that report positions of and within a window. Most of these functions report positions relative to an origin at the native position of the window’s frame (see [Frame Geometry](Frame-Geometry.html)). Some functions report positions relative to the origin of the display of the window’s frame. In any case, the origin has the coordinates (0, 0) and X and Y coordinates increase rightward and downward respectively.

For the following functions, X and Y coordinates are reported in integer character units, i.e., numbers of lines and columns respectively. On a graphical display, each “line” and “column” corresponds to the height and width of the default character specified by the frame’s default font (see [Frame Font](Frame-Font.html)).

*   Function: **window-edges** *\&optional window body absolute pixelwise*

    This function returns a list of the edge coordinates of `window`. If `window` is omitted or `nil`, it defaults to the selected window.

    The return value has the form `(left top right bottom)`. These list elements are, respectively, the X coordinate of the leftmost column occupied by the window, the Y coordinate of the topmost row, the X coordinate one column to the right of the rightmost column, and the Y coordinate one row down from the bottommost row.

    Note that these are the actual outer edges of the window, including any header line, mode line, scroll bar, fringes, window divider and display margins. On a text terminal, if the window has a neighbor on its right, its right edge includes the separator line between the window and its neighbor.

    If the optional argument `body` is `nil`, this means to return the edges corresponding to the total size of `window`. `body` non-`nil` means to return the edges of `window`’s body (aka text area). If `body` is non-`nil`, `window` must specify a live window.

    If the optional argument `absolute` is `nil`, this means to return edges relative to the native position of `window`’s frame. `absolute` non-`nil` means to return coordinates relative to the origin (0, 0) of `window`’s display. On non-graphical systems this argument has no effect.

    If the optional argument `pixelwise` is `nil`, this means to return the coordinates in terms of the default character width and height of `window`’s frame (see [Frame Font](Frame-Font.html)), rounded if necessary. `pixelwise` non-`nil` means to return the coordinates in pixels. Note that the pixel specified by `right` and `bottom` is immediately outside of these edges. If `absolute` is non-`nil`, `pixelwise` is implicitly non-`nil` too.

<!---->

*   Function: **window-body-edges** *\&optional window*

    This function returns the edges of `window`’s body (see [Window Sizes](Window-Sizes.html)). Calling `(window-body-edges window)` is equivalent to calling `(window-edges window t)`, see above.

The following functions can be used to relate a set of frame-relative coordinates to a window:

*   Function: **window-at** *x y \&optional frame*

    This function returns the live window at the coordinates `x` and `y` given in default character sizes (see [Frame Font](Frame-Font.html)) relative to the native position of `frame` (see [Frame Geometry](Frame-Geometry.html)).

    If there is no window at that position, the return value is `nil`. If `frame` is omitted or `nil`, it defaults to the selected frame.

<!---->

*   Function: **coordinates-in-window-p** *coordinates window*

    This function checks whether a window `window` occupies the frame relative coordinates `coordinates`, and if so, which part of the window that is. `window` should be a live window.

    `coordinates` should be a cons cell of the form `(x . y)`, where `x` and `y` are given in default character sizes (see [Frame Font](Frame-Font.html)) relative to the native position of `window`’s frame (see [Frame Geometry](Frame-Geometry.html)).

    If there is no window at the specified position, the return value is `nil` . Otherwise, the return value is one of the following:

    *   `(relx . rely)`

        The coordinates are inside `window`. The numbers `relx` and `rely` are the equivalent window-relative coordinates for the specified position, counting from 0 at the top left corner of the window.

    *   `mode-line`

        The coordinates are in the mode line of `window`.

    *   `header-line`

        The coordinates are in the header line of `window`.

    *   `tab-line`

        The coordinates are in the tab line of `window`.

    *   `right-divider`

        The coordinates are in the divider separating `window` from a window on the right.

    *   `bottom-divider`

        The coordinates are in the divider separating `window` from a window beneath.

    *   `vertical-line`

        The coordinates are in the vertical line between `window` and its neighbor to the right. This value occurs only if the window doesn’t have a scroll bar; positions in a scroll bar are considered outside the window for these purposes.

    *   *   `left-fringe`
        *   `right-fringe`

        The coordinates are in the left or right fringe of the window.

    *   *   `left-margin`
        *   `right-margin`

        The coordinates are in the left or right margin of the window.

    *   `nil`

        The coordinates are not in any part of `window`.

    The function `coordinates-in-window-p` does not require a frame as argument because it always uses the frame that `window` is on.

The following functions return window positions in pixels, rather than character units. Though mostly useful on graphical displays, they can also be called on text terminals, where the screen area of each text character is taken to be one pixel.

*   Function: **window-pixel-edges** *\&optional window*

    This function returns a list of pixel coordinates for the edges of `window`. Calling `(window-pixel-edges window)` is equivalent to calling `(window-edges window nil nil t)`, see above.

<!---->

*   Function: **window-body-pixel-edges** *\&optional window*

    This function returns the pixel edges of `window`’s body. Calling `(window-body-pixel-edges window)` is equivalent to calling `(window-edges window t nil t)`, see above.

The following functions return window positions in pixels, relative to the origin of the display screen rather than that of the frame:

*   Function: **window-absolute-pixel-edges** *\&optional window*

    This function returns the pixel coordinates of `window` relative to an origin at (0, 0) of the display of `window`’s frame. Calling `(window-absolute-pixel-edges)` is equivalent to calling `(window-edges window nil t t)`, see above.

<!---->

*   Function: **window-absolute-body-pixel-edges** *\&optional window*

    This function returns the pixel coordinates of `window`’s body relative to an origin at (0, 0) of the display of `window`’s frame. Calling `(window-absolute-body-pixel-edges window)` is equivalent to calling `(window-edges window t t t)`, see above.

    Combined with `set-mouse-absolute-pixel-position`, this function can be used to move the mouse pointer to an arbitrary buffer position visible in some window:

        (let ((edges (window-absolute-body-pixel-edges))
              (position (pos-visible-in-window-p nil nil t)))
          (set-mouse-absolute-pixel-position
           (+ (nth 0 edges) (nth 0 position))
           (+ (nth 1 edges) (nth 1 position))))

    On a graphical terminal this form “warps” the mouse cursor to the upper left corner of the glyph at the selected window’s point. A position calculated this way can be also used to show a tooltip window there.

The following function returns the screen coordinates of a buffer position visible in a window:

*   Function: **window-absolute-pixel-position** *\&optional position window*

    If the buffer position `position` is visible in window `window`, this function returns the display coordinates of the upper/left corner of the glyph at `position`. The return value is a cons of the X- and Y-coordinates of that corner, relative to an origin at (0, 0) of `window`’s display. It returns `nil` if `position` is not visible in `window`.

    `window` must be a live window and defaults to the selected window. `position` defaults to the value of `window-point` of `window`.

    This means that in order to move the mouse pointer to the position of point in the selected window, it’s sufficient to write:

        (let ((position (window-absolute-pixel-position)))
          (set-mouse-absolute-pixel-position
           (car position) (cdr position)))

The following function returns the largest rectangle that can be inscribed in a window without covering text displayed in that window.

*   Function: **window-largest-empty-rectangle** *\&optional window count min-width min-height positions left*

    This function calculates the dimensions of the largest empty rectangle that can be inscribed in the specified `window`’s text area. `window` must be a live window and defaults to the selected one.

    The return value is a triple of the width and the start and end y-coordinates of the largest rectangle that can be inscribed into the empty space (space not displaying any text) of the text area of `window`. No x-coordinates are returned by this function—any such rectangle is assumed to end at the right edge of `window`’s text area. If no empty space can be found, the return value is `nil`.

    The optional argument `count`, if non-`nil`, specifies a maximum number of rectangles to return. This means that the return value is a list of triples specifying rectangles with the largest rectangle first. `count` can be also a cons cell whose car specifies the number of rectangles to return and whose CDR, if non-`nil`, states that all rectangles returned must be disjoint.

    The optional arguments `min-width` and `min-height`, if non-`nil`, specify the minimum width and height of any rectangle returned.

    The optional argument `positions`, if non-`nil`, is a cons cell whose CAR specifies the uppermost and whose CDR specifies the lowermost pixel position that must be covered by any rectangle returned. These positions measure from the start of the text area of `window`.

    The optional argument `left`, if non-`nil`, means to return values suitable for buffers displaying right to left text. In that case, any rectangle returned is assumed to start at the left edge of `window`’s text area.

    Note that this function has to retrieve the dimensions of each line of `window`’s glyph matrix via `window-lines-pixel-dimensions` (see [Size of Displayed Text](Size-of-Displayed-Text.html)). Hence, this function may also return `nil` when the current glyph matrix of `window` is not up-to-date.

Next: [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html), Previous: [Horizontal Scrolling](Horizontal-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
