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

Next: [List Motion](List-Motion.html), Previous: [Text Lines](Text-Lines.html), Up: [Motion](Motion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 30.2.5 Motion by Screen Lines

The line functions in the previous section count text lines, delimited only by newline characters. By contrast, these functions count screen lines, which are defined by the way the text appears on the screen. A text line is a single screen line if it is short enough to fit the width of the selected window, but otherwise it may occupy several screen lines.

In some cases, text lines are truncated on the screen rather than continued onto additional screen lines. In these cases, `vertical-motion` moves point much like `forward-line`. See [Truncation](Truncation.html).

Because the width of a given string depends on the flags that control the appearance of certain characters, `vertical-motion` behaves differently, for a given piece of text, depending on the buffer it is in, and even on the selected window (because the width, the truncation flag, and display table may vary between windows). See [Usual Display](Usual-Display.html).

These functions scan text to determine where screen lines break, and thus take time proportional to the distance scanned.

*   Function: **vertical-motion** *count \&optional window cur-col*

    This function moves point to the start of the screen line `count` screen lines down from the screen line containing point. If `count` is negative, it moves up instead.

    The `count` argument can be a cons cell, `(cols . lines)`, instead of an integer. Then the function moves by `lines` screen lines, and puts point `cols` columns from the visual start of that screen line. Note that `cols` are counted from the *visual* start of the line; if the window is scrolled horizontally (see [Horizontal Scrolling](Horizontal-Scrolling.html)), the column on which point will end is in addition to the number of columns by which the text is scrolled.

    The return value is the number of screen lines over which point was moved. The value may be less in absolute value than `count` if the beginning or end of the buffer was reached.

    The window `window` is used for obtaining parameters such as the width, the horizontal scrolling, and the display table. But `vertical-motion` always operates on the current buffer, even if `window` currently displays some other buffer.

    The optional argument `cur-col` specifies the current column when the function is called. This is the window-relative horizontal coordinate of point, measured in units of font width of the frame’s default face. Providing it speeds up the function, especially in very long lines, because the function doesn’t have to go back in the buffer in order to determine the current column. Note that `cur-col` is also counted from the visual start of the line.

<!---->

*   Function: **count-screen-lines** *\&optional beg end count-final-newline window*

    This function returns the number of screen lines in the text from `beg` to `end`. The number of screen lines may be different from the number of actual lines, due to line continuation, the display table, etc. If `beg` and `end` are `nil` or omitted, they default to the beginning and end of the accessible portion of the buffer.

    If the region ends with a newline, that is ignored unless the optional third argument `count-final-newline` is non-`nil`.

    The optional fourth argument `window` specifies the window for obtaining parameters such as width, horizontal scrolling, and so on. The default is to use the selected window’s parameters.

    Like `vertical-motion`, `count-screen-lines` always uses the current buffer, regardless of which buffer is displayed in `window`. This makes possible to use `count-screen-lines` in any buffer, whether or not it is currently displayed in some window.

<!---->

*   Command: **move-to-window-line** *count*

    This function moves point with respect to the text currently displayed in the selected window. It moves point to the beginning of the screen line `count` screen lines from the top of the window; zero means the topmost line. If `count` is negative, that specifies a position -`count`<!-- /@w --> lines from the bottom (or the last line of the buffer, if the buffer ends above the specified screen position); thus, `count` of -1 specifies the last fully visible screen line of the window.

    If `count` is `nil`, then point moves to the beginning of the line in the middle of the window. If the absolute value of `count` is greater than the size of the window, then point moves to the place that would appear on that screen line if the window were tall enough. This will probably cause the next redisplay to scroll to bring that location onto the screen.

    In an interactive call, `count` is the numeric prefix argument.

    The value returned is the screen line number point has moved to, relative to the top line of the window.

<!---->

*   Function: **move-to-window-group-line** *count*

    This function is like `move-to-window-line`, except that when the selected window is a part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `move-to-window-group-line` will move to a position with respect to the entire group, not just the single window. This condition holds when the buffer local variable `move-to-window-group-line-function` is set to a function. In this case, `move-to-window-group-line` calls the function with the argument `count`, then returns its result.

<!---->

*   Function: **compute-motion** *from frompos to topos width offsets window*

    This function scans the current buffer, calculating screen positions. It scans the buffer forward from position `from`, assuming that is at screen coordinates `frompos`, to position `to` or coordinates `topos`, whichever comes first. It returns the ending buffer position and screen coordinates.

    The coordinate arguments `frompos` and `topos` are cons cells of the form `(hpos . vpos)`.

    The argument `width` is the number of columns available to display text; this affects handling of continuation lines. `nil` means the actual number of usable text columns in the window, which is equivalent to the value returned by `(window-width window)`.

    The argument `offsets` is either `nil` or a cons cell of the form `(hscroll . tab-offset)`. Here `hscroll` is the number of columns not being displayed at the left margin; most callers get this by calling `window-hscroll`. Meanwhile, `tab-offset` is the offset between column numbers on the screen and column numbers in the buffer. This can be nonzero in a continuation line, when the previous screen lines’ widths do not add up to a multiple of `tab-width`. It is always zero in a non-continuation line.

    The window `window` serves only to specify which display table to use. `compute-motion` always operates on the current buffer, regardless of what buffer is displayed in `window`.

    The return value is a list of five elements:

        (pos hpos vpos prevhpos contin)

    Here `pos` is the buffer position where the scan stopped, `vpos` is the vertical screen position, and `hpos` is the horizontal screen position.

    The result `prevhpos` is the horizontal position one character back from `pos`. The result `contin` is `t` if the last line was continued after (or within) the previous character.

    For example, to find the buffer position of column `col` of screen line `line` of a certain window, pass the window’s display start location as `from` and the window’s upper-left coordinates as `frompos`. Pass the buffer’s `(point-max)` as `to`, to limit the scan to the end of the accessible portion of the buffer, and pass `line` and `col` as `topos`. Here’s a function that does this:

        (defun coordinates-of-position (col line)
          (car (compute-motion (window-start)
                               '(0 . 0)
                               (point-max)
                               (cons col line)
                               (window-width)
                               (cons (window-hscroll) 0)
                               (selected-window))))

    When you use `compute-motion` for the minibuffer, you need to use `minibuffer-prompt-width` to get the horizontal position of the beginning of the first screen line. See [Minibuffer Contents](Minibuffer-Contents.html).

Next: [List Motion](List-Motion.html), Previous: [Text Lines](Text-Lines.html), Up: [Motion](Motion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
