

Next: [Deleting Windows](Deleting-Windows.html), Previous: [Preserving Window Sizes](Preserving-Window-Sizes.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.6 Splitting Windows

This section describes functions for creating a new window by *splitting* an existing one. Note that some windows are special in the sense that these functions may fail to split them as described here. Examples of such windows are side windows (see [Side Windows](Side-Windows.html)) and atomic windows (see [Atomic Windows](Atomic-Windows.html)).

*   Function: **split-window** *\&optional window size side pixelwise*

    This function creates a new live window next to the window `window`. If `window` is omitted or `nil`, it defaults to the selected window. That window is split, and reduced in size. The space is taken up by the new window, which is returned.

    The optional second argument `size` determines the sizes of `window` and/or the new window. If it is omitted or `nil`, both windows are given equal sizes; if there is an odd line, it is allocated to the new window. If `size` is a positive number, `window` is given `size` lines (or columns, depending on the value of `side`). If `size` is a negative number, the new window is given -`size` lines (or columns).

    If `size` is `nil`, this function obeys the variables `window-min-height` and `window-min-width` (see [Window Sizes](Window-Sizes.html)). Thus, it signals an error if splitting would result in making a window smaller than those variables specify. However, a non-`nil` value for `size` causes those variables to be ignored; in that case, the smallest allowable window is considered to be one that has space for a text area one line tall and/or two columns wide.

    Hence, if `size` is specified, it’s the caller’s responsibility to check whether the emanating windows are large enough to encompass all areas like a mode line or a scroll bar. The function `window-min-size` (see [Window Sizes](Window-Sizes.html)) can be used to determine the minimum requirements of `window` in this regard. Since the new window usually inherits areas like the mode line or the scroll bar from `window`, that function is also a good guess for the minimum size of the new window. The caller should specify a smaller size only if it correspondingly removes an inherited area before the next redisplay.

    The optional third argument `side` determines the position of the new window relative to `window`. If it is `nil` or `below`, the new window is placed below `window`. If it is `above`, the new window is placed above `window`. In both these cases, `size` specifies a total window height, in lines.

    If `side` is `t` or `right`, the new window is placed on the right of `window`. If `side` is `left`, the new window is placed on the left of `window`. In both these cases, `size` specifies a total window width, in columns.

    The optional fourth argument `pixelwise`, if non-`nil`, means to interpret `size` in units of pixels, instead of lines and columns.

    If `window` is a live window, the new window inherits various properties from it, including margins and scroll bars. If `window` is an internal window, the new window inherits the properties of the window selected within `window`’s frame.

    The behavior of this function may be altered by the window parameters of `window`, so long as the variable `ignore-window-parameters` is `nil`. If the value of the `split-window` window parameter is `t`, this function ignores all other window parameters. Otherwise, if the value of the `split-window` window parameter is a function, that function is called with the arguments `window`, `size`, and `side`, in lieu of the usual action of `split-window`. Otherwise, this function obeys the `window-atom` or `window-side` window parameter, if any. See [Window Parameters](Window-Parameters.html).

As an example, here is a sequence of `split-window` calls that yields the window configuration discussed in [Windows and Frames](Windows-and-Frames.html). This example demonstrates splitting a live window as well as splitting an internal window. We begin with a frame containing a single window (a live root window), which we denote by `W4`. Calling `(split-window W4)` yields this window configuration:

```lisp
     ______________________________________
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W4_________________||
    | ____________________________________ |
    ||                                    ||
    ||                                    ||
    ||                                    ||
    ||_________________W5_________________||
    |__________________W3__________________|
```

The `split-window` call has created a new live window, denoted by `W5`. It has also created a new internal window, denoted by `W3`, which becomes the root window and the parent of both `W4` and `W5`.

Next, we call `(split-window W3 nil 'left)`, passing the internal window `W3` as the argument. The result:

```lisp
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
```

A new live window `W2` is created, to the left of the internal window `W3`. A new internal window `W1` is created, becoming the new root window.

For interactive use, Emacs provides two commands which always split the selected window. These call `split-window` internally.

*   Command: **split-window-right** *\&optional size*

    This function splits the selected window into two side-by-side windows, putting the selected window on the left. If `size` is positive, the left window gets `size` columns; if `size` is negative, the right window gets -`size` columns.

<!---->

*   Command: **split-window-below** *\&optional size*

    This function splits the selected window into two windows, one above the other, leaving the upper window selected. If `size` is positive, the upper window gets `size` lines; if `size` is negative, the lower window gets -`size` lines.

<!---->

*   User Option: **split-window-keep-point**

    If the value of this variable is non-`nil` (the default), `split-window-below` behaves as described above.

    If it is `nil`, `split-window-below` adjusts point in each of the two windows to minimize redisplay. (This is useful on slow terminals.) It selects whichever window contains the screen line that point was previously on. Note that this only affects `split-window-below`, not the lower-level `split-window` function.

Next: [Deleting Windows](Deleting-Windows.html), Previous: [Preserving Window Sizes](Preserving-Window-Sizes.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
