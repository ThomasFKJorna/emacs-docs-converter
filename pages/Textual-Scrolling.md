

Next: [Vertical Scrolling](Vertical-Scrolling.html), Previous: [Window Start and End](Window-Start-and-End.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.21 Textual Scrolling

*Textual scrolling* means moving the text up or down through a window. It works by changing the window’s display-start location. It may also change the value of `window-point` to keep point on the screen (see [Window Point](Window-Point.html)).

The basic textual scrolling functions are `scroll-up` (which scrolls forward) and `scroll-down` (which scrolls backward). In these function names, “up” and “down” refer to the direction of motion of the buffer text relative to the window. Imagine that the text is written on a long roll of paper and that the scrolling commands move the paper up and down. Thus, if you are looking at the middle of a buffer and repeatedly call `scroll-down`, you will eventually see the beginning of the buffer.

Unfortunately, this sometimes causes confusion, because some people tend to think in terms of the opposite convention: they imagine the window moving over text that remains in place, so that “down” commands take you to the end of the buffer. This convention is consistent with fact that such a command is bound to a key named `PageDown` on modern keyboards.

Textual scrolling functions (aside from `scroll-other-window`) have unpredictable results if the current buffer is not the one displayed in the selected window. See [Current Buffer](Current-Buffer.html).

If the window contains a row taller than the height of the window (for example in the presence of a large image), the scroll functions will adjust the window’s vertical scroll position to scroll the partially visible row. Lisp callers can disable this feature by binding the variable `auto-window-vscroll` to `nil` (see [Vertical Scrolling](Vertical-Scrolling.html)).

*   Command: **scroll-up** *\&optional count*

    This function scrolls forward by `count` lines in the selected window.

    If `count` is negative, it scrolls backward instead. If `count` is `nil` (or omitted), the distance scrolled is `next-screen-context-lines` lines less than the height of the window’s text area.

    If the selected window cannot be scrolled any further, this function signals an error. Otherwise, it returns `nil`.

<!---->

*   Command: **scroll-down** *\&optional count*

    This function scrolls backward by `count` lines in the selected window.

    If `count` is negative, it scrolls forward instead. In other respects, it behaves the same way as `scroll-up` does.

<!---->

*   Command: **scroll-up-command** *\&optional count*

    This behaves like `scroll-up`, except that if the selected window cannot be scrolled any further and the value of the variable `scroll-error-top-bottom` is `t`, it tries to move to the end of the buffer instead. If point is already there, it signals an error.

<!---->

*   Command: **scroll-down-command** *\&optional count*

    This behaves like `scroll-down`, except that if the selected window cannot be scrolled any further and the value of the variable `scroll-error-top-bottom` is `t`, it tries to move to the beginning of the buffer instead. If point is already there, it signals an error.

<!---->

*   Command: **scroll-other-window** *\&optional count*

    This function scrolls the text in another window upward `count` lines. Negative values of `count`, or `nil`, are handled as in `scroll-up`.

    You can specify which buffer to scroll by setting the variable `other-window-scroll-buffer` to a buffer. If that buffer isn’t already displayed, `scroll-other-window` displays it in some window.

    When the selected window is the minibuffer, the next window is normally the leftmost one immediately above it. You can specify a different window to scroll, when the minibuffer is selected, by setting the variable `minibuffer-scroll-window`. This variable has no effect when any other window is selected. When it is non-`nil` and the minibuffer is selected, it takes precedence over `other-window-scroll-buffer`. See [Definition of minibuffer-scroll-window](Minibuffer-Misc.html#Definition-of-minibuffer_002dscroll_002dwindow).

    When the minibuffer is active, it is the next window if the selected window is the one at the bottom right corner. In this case, `scroll-other-window` attempts to scroll the minibuffer. If the minibuffer contains just one line, it has nowhere to scroll to, so the line reappears after the echo area momentarily displays the message ‘`End of buffer`’.

<!---->

*   Command: **scroll-other-window-down** *\&optional count*

    This function scrolls the text in another window downward `count` lines. Negative values of `count`, or `nil`, are handled as in `scroll-down`. In other respects, it behaves the same way as `scroll-other-window` does.

<!---->

*   Variable: **other-window-scroll-buffer**

    If this variable is non-`nil`, it tells `scroll-other-window` which buffer’s window to scroll.

<!---->

*   User Option: **scroll-margin**

    This option specifies the size of the scroll margin—a minimum number of lines between point and the top or bottom of a window. Whenever point gets within this many lines of the top or bottom of the window, redisplay scrolls the text automatically (if possible) to move point out of the margin, closer to the center of the window.

<!---->

*   User Option: **maximum-scroll-margin**

    This variable limits the effective value of `scroll-margin` to a fraction of the current window line height. For example, if the current window has 20 lines and `maximum-scroll-margin` is 0.1, then the scroll margins will never be larger than 2 lines, no matter how big `scroll-margin` is.

    `maximum-scroll-margin` itself has a maximum value of 0.5, which allows setting margins large to keep the cursor at the middle line of the window (or two middle lines if the window has an even number of lines). If it’s set to a larger value (or any value other than a float between 0.0 and 0.5) then the default value of 0.25 will be used instead.

<!---->

*   User Option: **scroll-conservatively**

    This variable controls how scrolling is done automatically when point moves off the screen (or into the scroll margin). If the value is a positive integer `n`, then redisplay scrolls the text up to `n` lines in either direction, if that will bring point back into proper view. This behavior is called *conservative scrolling*. Otherwise, scrolling happens in the usual way, under the control of other variables such as `scroll-up-aggressively` and `scroll-down-aggressively`.

    The default value is zero, which means that conservative scrolling never happens.

<!---->

*   User Option: **scroll-down-aggressively**

    The value of this variable should be either `nil` or a fraction `f` between 0 and 1. If it is a fraction, that specifies where on the screen to put point when scrolling down. More precisely, when a window scrolls down because point is above the window start, the new start position is chosen to put point `f` part of the window height from the top. The larger `f`, the more aggressive the scrolling.

    A value of `nil` is equivalent to .5, since its effect is to center point. This variable automatically becomes buffer-local when set in any fashion.

<!---->

*   User Option: **scroll-up-aggressively**

    Likewise, for scrolling up. The value, `f`, specifies how far point should be placed from the bottom of the window; thus, as with `scroll-down-aggressively`, a larger value scrolls more aggressively.

<!---->

*   User Option: **scroll-step**

    This variable is an older variant of `scroll-conservatively`. The difference is that if its value is `n`, that permits scrolling only by precisely `n` lines, not a smaller number. This feature does not work with `scroll-margin`. The default value is zero.

<!---->

*   User Option: **scroll-preserve-screen-position**

    If this option is `t`, whenever a scrolling command moves point off-window, Emacs tries to adjust point to keep the cursor at its old vertical position in the window, rather than the window edge.

    If the value is non-`nil` and not `t`, Emacs adjusts point to keep the cursor at the same vertical position, even if the scrolling command didn’t move point off-window.

    This option affects all scroll commands that have a non-`nil` `scroll-command` symbol property.

<!---->

*   User Option: **next-screen-context-lines**

    The value of this variable is the number of lines of continuity to retain when scrolling by full screens. For example, `scroll-up` with an argument of `nil` scrolls so that this many lines at the bottom of the window appear instead at the top. The default value is `2`.

<!---->

*   User Option: **scroll-error-top-bottom**

    If this option is `nil` (the default), `scroll-up-command` and `scroll-down-command` simply signal an error when no more scrolling is possible.

    If the value is `t`, these commands instead move point to the beginning or end of the buffer (depending on scrolling direction); only if point is already on that position do they signal an error.

<!---->

*   Command: **recenter** *\&optional count redisplay*

    This function scrolls the text in the selected window so that point is displayed at a specified vertical position within the window. It does not move point with respect to the text.

    If `count` is a non-negative number, that puts the line containing point `count` lines down from the top of the window. If `count` is a negative number, then it counts upward from the bottom of the window, so that -1 stands for the last usable line in the window.

    If `count` is `nil` (or a non-`nil` list), `recenter` puts the line containing point in the middle of the window. If `count` is `nil` and `redisplay` is non-`nil`, this function may redraw the frame, according to the value of `recenter-redisplay`. Thus, omitting the second argument can be used to countermand the effect of `recenter-redisplay` being non-`nil`. Interactive calls pass non-‘nil’ for `redisplay`.

    When `recenter` is called interactively, `count` is the raw prefix argument. Thus, typing `C-u` as the prefix sets the `count` to a non-`nil` list, while typing `C-u 4` sets `count` to 4, which positions the current line four lines from the top.

    With an argument of zero, `recenter` positions the current line at the top of the window. The command `recenter-top-bottom` offers a more convenient way to achieve this.

<!---->

*   Function: **recenter-window-group** *\&optional count*

    This function is like `recenter`, except that when the selected window is part of a group of windows (see [Window Group](Basic-Windows.html#Window-Group)), `recenter-window-group` scrolls the entire group. This condition holds when the buffer local variable `recenter-window-group-function` is set to a function. In this case, `recenter-window-group` calls the function with the argument `count`, then returns its result. The argument `count` has the same meaning as in `recenter`, but with respect to the entire window group.

<!---->

*   User Option: **recenter-redisplay**

    If this variable is non-`nil`, calling `recenter` with a `nil` `count` argument and non-`nil` `redisplay` argument redraws the frame. The default value is `tty`, which means only redraw the frame if it is a tty frame.

<!---->

*   Command: **recenter-top-bottom** *\&optional count*

    This command, which is the default binding for `C-l`, acts like `recenter`, except if called with no argument. In that case, successive calls place point according to the cycling order defined by the variable `recenter-positions`.

<!---->

*   User Option: **recenter-positions**

    This variable controls how `recenter-top-bottom` behaves when called with no argument. The default value is `(middle top bottom)`, which means that successive calls of `recenter-top-bottom` with no argument cycle between placing point at the middle, top, and bottom of the window.

Next: [Vertical Scrolling](Vertical-Scrolling.html), Previous: [Window Start and End](Window-Start-and-End.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
