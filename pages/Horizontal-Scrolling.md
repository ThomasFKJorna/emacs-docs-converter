

Next: [Coordinates and Windows](Coordinates-and-Windows.html), Previous: [Vertical Scrolling](Vertical-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.23 Horizontal Scrolling

*Horizontal scrolling* means shifting the image in the window left or right by a specified multiple of the normal character width. Each window has a *horizontal scroll position*, which is a number, never less than zero. It specifies how far to shift the contents left. Shifting the window contents left generally makes all or part of some characters disappear off the left, and all or part of some other characters appear at the right. The usual value is zero.

The horizontal scroll position is measured in units of the normal character width, which is the width of space in the default font. Thus, if the value is 5, that means the window contents are scrolled left by 5 times the normal character width. How many characters actually disappear off to the left depends on their width, and could vary from line to line.

Because we read from side to side in the inner loop, and from top to bottom in the outer loop, the effect of horizontal scrolling is not like that of textual or vertical scrolling. Textual scrolling involves selection of a portion of text to display, and vertical scrolling moves the window contents contiguously; but horizontal scrolling causes part of *each line* to go off screen.

Usually, no horizontal scrolling is in effect; then the leftmost column is at the left edge of the window. In this state, scrolling to the right is meaningless, since there is no data to the left of the edge to be revealed by it; so this is not allowed. Scrolling to the left is allowed; it scrolls the first columns of text off the edge of the window and can reveal additional columns on the right that were truncated before. Once a window has a nonzero amount of leftward horizontal scrolling, you can scroll it back to the right, but only so far as to reduce the net horizontal scroll to zero. There is no limit to how far left you can scroll, but eventually all the text will disappear off the left edge.

If `auto-hscroll-mode` is set, redisplay automatically alters the horizontal scrolling of a window as necessary to ensure that point is always visible. However, you can still set the horizontal scrolling value explicitly. The value you specify serves as a lower bound for automatic scrolling, i.e., automatic scrolling will not scroll a window to a column less than the specified one.

The default value of `auto-hscroll-mode` is `t`; setting it to `current-line` activates a variant of automatic horizontal scrolling whereby only the line showing the cursor is horizontally scrolled to make point visible, the rest of the window is left either unscrolled, or at the minimum scroll amount set by `scroll-left` and `scroll-right`, see below.

*   Command: **scroll-left** *\&optional count set-minimum*

    This function scrolls the selected window `count` columns to the left (or to the right if `count` is negative). The default for `count` is the window width, minus 2.

    The return value is the total amount of leftward horizontal scrolling in effect after the change—just like the value returned by `window-hscroll` (below).

    Note that text in paragraphs whose base direction is right-to-left (see [Bidirectional Display](Bidirectional-Display.html)) moves in the opposite direction: e.g., it moves to the right when `scroll-left` is invoked with a positive value of `count`.

    Once you scroll a window as far right as it can go, back to its normal position where the total leftward scrolling is zero, attempts to scroll any farther right have no effect.

    If `set-minimum` is non-`nil`, the new scroll amount becomes the lower bound for automatic scrolling; that is, automatic scrolling will not scroll a window to a column less than the value returned by this function. Interactive calls pass non-`nil` for `set-minimum`.

<!---->

*   Command: **scroll-right** *\&optional count set-minimum*

    This function scrolls the selected window `count` columns to the right (or to the left if `count` is negative). The default for `count` is the window width, minus 2. Aside from the direction of scrolling, this works just like `scroll-left`.

<!---->

*   Function: **window-hscroll** *\&optional window*

    This function returns the total leftward horizontal scrolling of `window`—the number of columns by which the text in `window` is scrolled left past the left margin. (In right-to-left paragraphs, the value is the total amount of the rightward scrolling instead.) The default for `window` is the selected window.

    The return value is never negative. It is zero when no horizontal scrolling has been done in `window` (which is usually the case).

    ```lisp
    (window-hscroll)
         ⇒ 0
    ```

    ```lisp
    (scroll-left 5)
         ⇒ 5
    ```

    ```lisp
    (window-hscroll)
         ⇒ 5
    ```

<!---->

*   Function: **set-window-hscroll** *window columns*

    This function sets horizontal scrolling of `window`. The value of `columns` specifies the amount of scrolling, in terms of columns from the left margin (right margin in right-to-left paragraphs). The argument `columns` should be zero or positive; if not, it is taken as zero. Fractional values of `columns` are not supported at present.

    Note that `set-window-hscroll` may appear not to work if you test it by evaluating a call with `M-:` in a simple way. What happens is that the function sets the horizontal scroll value and returns, but then redisplay adjusts the horizontal scrolling to make point visible, and this overrides what the function did. You can observe the function’s effect if you call it while point is sufficiently far from the left margin that it will remain visible.

    The value returned is `columns`.

    ```lisp
    (set-window-hscroll (selected-window) 10)
         ⇒ 10
    ```

Here is how you can determine whether a given position `position` is off the screen due to horizontal scrolling:

```lisp
(defun hscroll-on-screen (window position)
  (save-excursion
    (goto-char position)
    (and
     (>= (- (current-column) (window-hscroll window)) 0)
     (< (- (current-column) (window-hscroll window))
        (window-width window)))))
```

Next: [Coordinates and Windows](Coordinates-and-Windows.html), Previous: [Vertical Scrolling](Vertical-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
