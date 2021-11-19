

Next: [Horizontal Scrolling](Horizontal-Scrolling.html), Previous: [Textual Scrolling](Textual-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.22 Vertical Fractional Scrolling

*Vertical fractional scrolling* means shifting text in a window up or down by a specified multiple or fraction of a line. Emacs uses it, for example, on images and screen lines which are taller than the window. Each window has a *vertical scroll position*, which is a number, never less than zero. It specifies how far to raise the contents of the window when displaying them. Raising the window contents generally makes all or part of some lines disappear off the top, and all or part of some other lines appear at the bottom. The usual value is zero.

The vertical scroll position is measured in units of the normal line height, which is the height of the default font. Thus, if the value is .5, that means the window contents will be scrolled up half the normal line height. If it is 3.3, that means the window contents are scrolled up somewhat over three times the normal line height.

What fraction of a line the vertical scrolling covers, or how many lines, depends on what the lines contain. A value of .5 could scroll a line whose height is very short off the screen, while a value of 3.3 could scroll just part of the way through a tall line or an image.

*   Function: **window-vscroll** *\&optional window pixels-p*

    This function returns the current vertical scroll position of `window`. The default for `window` is the selected window. If `pixels-p` is non-`nil`, the return value is measured in pixels, rather than in units of the normal line height.

    ```lisp
    (window-vscroll)
         ⇒ 0
    ```

<!---->

*   Function: **set-window-vscroll** *window lines \&optional pixels-p*

    This function sets `window`’s vertical scroll position to `lines`. If `window` is `nil`, the selected window is used. The argument `lines` should be zero or positive; if not, it is taken as zero.

    The actual vertical scroll position must always correspond to an integral number of pixels, so the value you specify is rounded accordingly.

    The return value is the result of this rounding.

    ```lisp
    (set-window-vscroll (selected-window) 1.2)
         ⇒ 1.13
    ```

    If `pixels-p` is non-`nil`, `lines` specifies a number of pixels. In this case, the return value is `lines`.

<!---->

*   Variable: **auto-window-vscroll**

    If this variable is non-`nil`, the `line-move`, `scroll-up`, and `scroll-down` functions will automatically modify the vertical scroll position to scroll through display rows that are taller than the height of the window, for example in the presence of large images.

Next: [Horizontal Scrolling](Horizontal-Scrolling.html), Previous: [Textual Scrolling](Textual-Scrolling.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
