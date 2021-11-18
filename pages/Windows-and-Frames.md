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

Next: [Window Sizes](Window-Sizes.html), Previous: [Basic Windows](Basic-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.2 Windows and Frames

Each window belongs to exactly one frame (see [Frames](Frames.html)).

*   Function: **window-frame** *\&optional window*

    This function returns the frame that the window `window` belongs to. If `window` is `nil`, it defaults to the selected window.

<!---->

*   Function: **window-list** *\&optional frame minibuffer window*

    This function returns a list of live windows belonging to the frame `frame`. If `frame` is omitted or `nil`, it defaults to the selected frame.

    The optional argument `minibuffer` specifies whether to include the minibuffer window in the returned list. If `minibuffer` is `t`, the minibuffer window is included. If `minibuffer` is `nil` or omitted, the minibuffer window is included only if it is active. If `minibuffer` is neither `nil` nor `t`, the minibuffer window is never included.

    The optional argument `window`, if non-`nil`, should be a live window on the specified frame; then `window` will be the first element in the returned list. If `window` is omitted or `nil`, the window selected within the frame is the first element.

Windows in the same frame are organized into a *window tree*, whose leaf nodes are the live windows. The internal nodes of a window tree are not live; they exist for the purpose of organizing the relationships between live windows. The root node of a window tree is called the *root window*. It can be either a live window (if the frame has just one window), or an internal window.

A minibuffer window (see [Minibuffer Windows](Minibuffer-Windows.html)) that is not alone on its frame does not have a parent window, so it strictly speaking is not part of its frame’s window tree. Nonetheless, it is a sibling window of the frame’s root window, and thus can be reached via `window-next-sibling`. Also, the function `window-tree` described at the end of this section lists the minibuffer window alongside the actual window tree.

*   Function: **frame-root-window** *\&optional frame-or-window*

    This function returns the root window for `frame-or-window`. The argument `frame-or-window` should be either a window or a frame; if omitted or `nil`, it defaults to the selected frame. If `frame-or-window` is a window, the return value is the root window of that window’s frame.

When a window is split, there are two live windows where previously there was one. One of these is represented by the same Lisp window object as the original window, and the other is represented by a newly-created Lisp window object. Both of these live windows become leaf nodes of the window tree, as *child windows* of a single internal window. If necessary, Emacs automatically creates this internal window, which is also called the *parent window*, and assigns it to the appropriate position in the window tree. A set of windows that share the same parent are called *siblings*.

*   Function: **window-parent** *\&optional window*

    This function returns the parent window of `window`. If `window` is omitted or `nil`, it defaults to the selected window. The return value is `nil` if `window` has no parent (i.e., it is a minibuffer window or the root window of its frame).

Each internal window always has at least two child windows. If this number falls to one as a result of window deletion, Emacs automatically deletes the internal window, and its sole remaining child window takes its place in the window tree.

Each child window can be either a live window, or an internal window (which in turn would have its own child windows). Therefore, each internal window can be thought of as occupying a certain rectangular *screen area*—the union of the areas occupied by the live windows that are ultimately descended from it.

For each internal window, the screen areas of the immediate children are arranged either vertically or horizontally (never both). If the child windows are arranged one above the other, they are said to form a *vertical combination*; if they are arranged side by side, they are said to form a *horizontal combination*. Consider the following example:

         ______________________________________
        | ______  ____________________________ |
        ||      || __________________________ ||
        ||      |||                          |||
        ||      |||                          |||
        ||      |||                          |||
        ||      |||____________W4____________|||
        ||      || __________________________ ||
        ||      |||                          |||
        ||      |||                          |||
        ||      |||____________W5____________|||
        ||__W2__||_____________W3_____________ |
        |__________________W1__________________|

The root window of this frame is an internal window, `W1`. Its child windows form a horizontal combination, consisting of the live window `W2` and the internal window `W3`. The child windows of `W3` form a vertical combination, consisting of the live windows `W4` and `W5`. Hence, the live windows in this window tree are `W2`, `W4`, and `W5`.

The following functions can be used to retrieve a child window of an internal window, and the siblings of a child window.

*   Function: **window-top-child** *\&optional window*

    This function returns the topmost child window of `window`, if `window` is an internal window whose children form a vertical combination. For any other type of window, the return value is `nil`.

<!---->

*   Function: **window-left-child** *\&optional window*

    This function returns the leftmost child window of `window`, if `window` is an internal window whose children form a horizontal combination. For any other type of window, the return value is `nil`.

<!---->

*   Function: **window-child** *window*

    This function returns the first child window of the internal window `window`—the topmost child window for a vertical combination, or the leftmost child window for a horizontal combination. If `window` is a live window, the return value is `nil`.

<!---->

*   Function: **window-combined-p** *\&optional window horizontal*

    This function returns a non-`nil` value if and only if `window` is part of a vertical combination. If `window` is omitted or `nil`, it defaults to the selected one.

    If the optional argument `horizontal` is non-`nil`, this means to return non-`nil` if and only if `window` is part of a horizontal combination.

<!---->

*   Function: **window-next-sibling** *\&optional window*

    This function returns the next sibling of the window `window`. If omitted or `nil`, `window` defaults to the selected window. The return value is `nil` if `window` is the last child of its parent.

<!---->

*   Function: **window-prev-sibling** *\&optional window*

    This function returns the previous sibling of the window `window`. If omitted or `nil`, `window` defaults to the selected window. The return value is `nil` if `window` is the first child of its parent.

The functions `window-next-sibling` and `window-prev-sibling` should not be confused with the functions `next-window` and `previous-window`, which return the next and previous window, respectively, in the cyclic ordering of windows (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)).

The following functions can be useful to locate a window within its frame.

*   Function: **frame-first-window** *\&optional frame-or-window*

    This function returns the live window at the upper left corner of the frame specified by `frame-or-window`. The argument `frame-or-window` must denote a window or a live frame and defaults to the selected frame. If `frame-or-window` specifies a window, this function returns the first window on that window’s frame. Under the assumption that the frame from our canonical example is selected `(frame-first-window)` returns `W2`.

<!---->

*   Function: **window-at-side-p** *\&optional window side*

    This function returns `t` if `window` is located at `side` of its containing frame. The argument `window` must be a valid window and defaults to the selected one. The argument `side` can be any of the symbols `left`, `top`, `right` or `bottom`. The default value `nil` is handled like `bottom`.

    Note that this function disregards the minibuffer window (see [Minibuffer Windows](Minibuffer-Windows.html)). Hence, with `side` equal to `bottom` it may return `t` also when the minibuffer window appears right below `window`.

<!---->

*   Function: **window-in-direction** *direction \&optional window ignore sign wrap mini*

    This function returns the nearest live window in direction `direction` as seen from the position of `window-point` in window `window`. The argument `direction` must be one of `above`, `below`, `left` or `right`. The optional argument `window` must denote a live window and defaults to the selected one.

    This function does not return a window whose `no-other-window` parameter is non-`nil` (see [Window Parameters](Window-Parameters.html)). If the nearest window’s `no-other-window` parameter is non-`nil`, this function tries to find another window in the indicated direction whose `no-other-window` parameter is `nil`. If the optional argument `ignore` is non-`nil`, a window may be returned even if its `no-other-window` parameter is non-`nil`.

    If the optional argument `sign` is a negative number, it means to use the right or bottom edge of `window` as reference position instead of `window-point`. If `sign` is a positive number, it means to use the left or top edge of `window` as reference position.

    If the optional argument `wrap` is non-`nil`, this means to wrap `direction` around frame borders. For example, if `window` is at the top of the frame and `direction` is `above`, then this function usually returns the frame’s minibuffer window if it’s active and a window at the bottom of the frame otherwise.

    If the optional argument `mini` is `nil`, this means to return the minibuffer window if and only if it is currently active. If `mini` is non-`nil`, this function may return the minibuffer window even when it’s not active. However, if `wrap` is non-`nil`, it always acts as if `mini` were `nil`.

    If it doesn’t find a suitable window, this function returns `nil`.

    Don’t use this function to check whether there is *no* window in `direction`. Calling `window-at-side-p` described above is a much more efficient way to do that.

The following function allows the entire window tree of a frame to be retrieved:

*   Function: **window-tree** *\&optional frame*

    This function returns a list representing the window tree for frame `frame`. If `frame` is omitted or `nil`, it defaults to the selected frame.

    The return value is a list of the form `(root mini)`, where `root` represents the window tree of the frame’s root window, and `mini` is the frame’s minibuffer window.

    If the root window is live, `root` is that window itself. Otherwise, `root` is a list `(dir edges w1 w2 ...)` where `dir` is `nil` for a horizontal combination and `t` for a vertical combination, `edges` gives the size and position of the combination, and the remaining elements are the child windows. Each child window may again be a window object (for a live window) or a list with the same format as above (for an internal window). The `edges` element is a list `(left top right bottom)`, similar to the value returned by `window-edges` (see [Coordinates and Windows](Coordinates-and-Windows.html)).

Next: [Window Sizes](Window-Sizes.html), Previous: [Basic Windows](Basic-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
