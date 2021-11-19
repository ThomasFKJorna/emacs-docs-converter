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

Next: [Textual Scrolling](Textual-Scrolling.html), Previous: [Window Point](Window-Point.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.20 The Window Start and End Positions

Each window maintains a marker used to keep track of a buffer position that specifies where in the buffer display should start. This position is called the *display-start* position of the window (or just the *start*). The character after this position is the one that appears at the upper left corner of the window. It is usually, but not inevitably, at the beginning of a text line.

After switching windows or buffers, and in some other cases, if the window start is in the middle of a line, Emacs adjusts the window start to the start of a line. This prevents certain operations from leaving the window start at a meaningless point within a line. This feature may interfere with testing some Lisp code by executing it using the commands of Lisp mode, because they trigger this readjustment. To test such code, put it into a command and bind the command to a key.

*   Function: **window-start** *\&optional window*

    This function returns the display-start position of window `window`. If `window` is `nil`, the selected window is used.

    When you create a window, or display a different buffer in it, the display-start position is set to a display-start position recently used for the same buffer, or to `point-min` if the buffer doesn’t have any.

    Redisplay updates the window-start position (if you have not specified it explicitly since the previous redisplay)—to make sure point appears on the screen. Nothing except redisplay automatically changes the window-start position; if you move point, do not expect the window-start position to change in response until after the next redisplay.

<!---->

*   Function: **window-group-start** *\&optional window*

    This function is like `window-start`, except that when `window` is a part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `window-group-start` returns the start position of the entire group. This condition holds when the buffer local variable `window-group-start-function` is set to a function. In this case, `window-group-start` calls the function with the single argument `window`, then returns its result.

<!---->

*   Function: **window-end** *\&optional window update*

    This function returns the position where display of its buffer ends in `window`. The default for `window` is the selected window.

    Simply changing the buffer text or moving point does not update the value that `window-end` returns. The value is updated only when Emacs redisplays and redisplay completes without being preempted.

    If the last redisplay of `window` was preempted, and did not finish, Emacs does not know the position of the end of display in that window. In that case, this function returns `nil`.

    If `update` is non-`nil`, `window-end` always returns an up-to-date value for where display ends, based on the current `window-start` value. If a previously saved value of that position is still valid, `window-end` returns that value; otherwise it computes the correct value by scanning the buffer text.

    Even if `update` is non-`nil`, `window-end` does not attempt to scroll the display if point has moved off the screen, the way real redisplay would do. It does not alter the `window-start` value. In effect, it reports where the displayed text will end if scrolling is not required. Note that the position it returns might be only partially visible.

<!---->

*   Function: **window-group-end** *\&optional window update*

    This function is like `window-end`, except that when `window` is a part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `window-group-end` returns the end position of the entire group. This condition holds when the buffer local variable `window-group-end-function` is set to a function. In this case, `window-group-end` calls the function with the two arguments `window` and `update`, then returns its result. The argument `update` has the same meaning as in `window-end`.

<!---->

*   Function: **set-window-start** *window position \&optional noforce*

    This function sets the display-start position of `window` to `position` in `window`’s buffer. It returns `position`.

    The display routines insist that the position of point be visible when a buffer is displayed. Normally, they select the display-start position according to their internal logic (and scroll the window if necessary) to make point visible. However, if you specify the start position with this function using `nil` for `noforce`, it means you want display to start at `position` even if that would put the location of point off the screen. If this does place point off screen, the display routines attempt to move point to the left margin on the middle line in the window.

    For example, if point is 1<!-- /@w --> and you set the start of the window to 37<!-- /@w -->, the start of the next line, point will be above the top of the window. The display routines will automatically move point if it is still 1 when redisplay occurs. Here is an example:

        ;; Here is what ‘foo’ looks like before executing
        ;;   the set-window-start expression.

    ```
    ```

        ---------- Buffer: foo ----------
        ∗This is the contents of buffer foo.
        2
        3
        4
        5
        6
        ---------- Buffer: foo ----------

    ```
    ```

        (set-window-start
         (selected-window)
         (save-excursion
           (goto-char 1)
           (forward-line 1)
           (point)))
        ⇒ 37

    ```
    ```

        ;; Here is what ‘foo’ looks like after executing
        ;;   the set-window-start expression.
        ---------- Buffer: foo ----------
        2
        3
        ∗4
        5
        6
        ---------- Buffer: foo ----------

    If the attempt to make point visible (i.e., in a fully-visible screen line) fails, the display routines will disregard the requested window-start position and compute a new one anyway. Thus, for reliable results Lisp programs that call this function should always move point to be inside the window whose display starts at `position`.

    If `noforce` is non-`nil`, and `position` would place point off screen at the next redisplay, then redisplay computes a new window-start position that works well with point, and thus `position` is not used.

<!---->

*   Function: **set-window-group-start** *window position \&optional noforce*

    This function is like `set-window-start`, except that when `window` is a part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `set-window-group-start` sets the start position of the entire group. This condition holds when the buffer local variable `set-window-group-start-function` is set to a function. In this case, `set-window-group-start` calls the function with the three arguments `window`, `position`, and `noforce`, then returns its result. The arguments `position` and `noforce` in this function have the same meaning as in `set-window-start`.

<!---->

*   Function: **pos-visible-in-window-p** *\&optional position window partially*

    This function returns non-`nil` if `position` is within the range of text currently visible on the screen in `window`. It returns `nil` if `position` is scrolled vertically out of view. Locations that are partially obscured are not considered visible unless `partially` is non-`nil`. The argument `position` defaults to the current position of point in `window`; `window` defaults to the selected window. If `position` is `t`, that means to check either the first visible position of the last screen line in `window`, or the end-of-buffer position, whichever comes first.

    This function considers only vertical scrolling. If `position` is out of view only because `window` has been scrolled horizontally, `pos-visible-in-window-p` returns non-`nil` anyway. See [Horizontal Scrolling](Horizontal-Scrolling.html).

    If `position` is visible, `pos-visible-in-window-p` returns `t` if `partially` is `nil`; if `partially` is non-`nil`, and the character following `position` is fully visible, it returns a list of the form `(x y)`, where `x` and `y` are the pixel coordinates relative to the top left corner of the window; otherwise it returns an extended list of the form `(x y rtop rbot rowh vpos)`, where `rtop` and `rbot` specify the number of off-window pixels at the top and bottom of the row at `position`, `rowh` specifies the visible height of that row, and `vpos` specifies the vertical position (zero-based row number) of that row.

    Here is an example:

        ;; If point is off the screen now, recenter it now.
        (or (pos-visible-in-window-p
             (point) (selected-window))
            (recenter 0))

<!---->

*   Function: **pos-visible-in-window-group-p** *\&optional position window partially*

    This function is like `pos-visible-in-window-p`, except that when `window` is a part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `pos-visible-in-window-group-p` tests the visibility of `pos` in the entire group, not just in the single `window`. This condition holds when the buffer local variable `pos-visible-in-window-group-p-function` is set to a function. In this case `pos-visible-in-window-group-p` calls the function with the three arguments `position`, `window`, and `partially`, then returns its result. The arguments `position` and `partially` have the same meaning as in `pos-visible-in-window-p`.

<!---->

*   Function: **window-line-height** *\&optional line window*

    This function returns the height of text line `line` in `window`. If `line` is one of `header-line` or `mode-line`, `window-line-height` returns information about the corresponding line of the window. Otherwise, `line` is a text line number starting from 0. A negative number counts from the end of the window. The default for `line` is the current line in `window`; the default for `window` is the selected window.

    If the display is not up to date, `window-line-height` returns `nil`. In that case, `pos-visible-in-window-p` may be used to obtain related information.

    If there is no line corresponding to the specified `line`, `window-line-height` returns `nil`. Otherwise, it returns a list `(height vpos ypos offbot)`, where `height` is the height in pixels of the visible part of the line, `vpos` and `ypos` are the vertical position in lines and pixels of the line relative to the top of the first text line, and `offbot` is the number of off-window pixels at the bottom of the text line. If there are off-window pixels at the top of the (first) text line, `ypos` is negative.

Next: [Textual Scrolling](Textual-Scrolling.html), Previous: [Window Point](Window-Point.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
