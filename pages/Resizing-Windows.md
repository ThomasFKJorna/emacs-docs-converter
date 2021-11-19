

Next: [Preserving Window Sizes](Preserving-Window-Sizes.html), Previous: [Window Sizes](Window-Sizes.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.4 Resizing Windows

This section describes functions for resizing a window without changing the size of its frame. Because live windows do not overlap, these functions are meaningful only on frames that contain two or more windows: resizing a window also changes the size of a neighboring window. If there is just one window on a frame, its size cannot be changed except by resizing the frame (see [Frame Size](Frame-Size.html)).

Except where noted, these functions also accept internal windows as arguments. Resizing an internal window causes its child windows to be resized to fit the same space.

*   Function: **window-resizable** *window delta \&optional horizontal ignore pixelwise*

    This function returns `delta` if the size of `window` can be changed vertically by `delta` lines. If the optional argument `horizontal` is non-`nil`, it instead returns `delta` if `window` can be resized horizontally by `delta` columns. It does not actually change the window size.

    If `window` is `nil`, it defaults to the selected window.

    A positive value of `delta` means to check whether the window can be enlarged by that number of lines or columns; a negative value of `delta` means to check whether the window can be shrunk by that many lines or columns. If `delta` is non-zero, a return value of 0 means that the window cannot be resized.

    Normally, the variables `window-min-height` and `window-min-width` specify the smallest allowable window size (see [Window Sizes](Window-Sizes.html)). However, if the optional argument `ignore` is non-`nil`, this function ignores `window-min-height` and `window-min-width`, as well as `window-size-fixed`. Instead, it considers the minimum-height window to be one consisting of a header and a mode line, a horizontal scrollbar and a bottom divider (if any), plus a text area one line tall; and a minimum-width window as one consisting of fringes, margins, a scroll bar and a right divider (if any), plus a text area two columns wide.

    If the optional argument `pixelwise` is non-`nil`, `delta` is interpreted as pixels.

<!---->

*   Function: **window-resize** *window delta \&optional horizontal ignore pixelwise*

    This function resizes `window` by `delta` increments. If `horizontal` is `nil`, it changes the height by `delta` lines; otherwise, it changes the width by `delta` columns. A positive `delta` means to enlarge the window, and a negative `delta` means to shrink it.

    If `window` is `nil`, it defaults to the selected window. If the window cannot be resized as demanded, an error is signaled.

    The optional argument `ignore` has the same meaning as for the function `window-resizable` above.

    If the optional argument `pixelwise` is non-`nil`, `delta` will be interpreted as pixels.

    The choice of which window edges this function alters depends on the values of the option `window-combination-resize` and the combination limits of the involved windows; in some cases, it may alter both edges. See [Recombining Windows](Recombining-Windows.html). To resize by moving only the bottom or right edge of a window, use the function `adjust-window-trailing-edge`.

<!---->

*   Function: **adjust-window-trailing-edge** *window delta \&optional horizontal pixelwise*

    This function moves `window`’s bottom edge by `delta` lines. If optional argument `horizontal` is non-`nil`, it instead moves the right edge by `delta` columns. If `window` is `nil`, it defaults to the selected window.

    If the optional argument `pixelwise` is non-`nil`, `delta` is interpreted as pixels.

    A positive `delta` moves the edge downwards or to the right; a negative `delta` moves it upwards or to the left. If the edge cannot be moved as far as specified by `delta`, this function moves it as far as possible but does not signal an error.

    This function tries to resize windows adjacent to the edge that is moved. If this is not possible for some reason (e.g., if that adjacent window is fixed-size), it may resize other windows.

<!---->

*   User Option: **window-resize-pixelwise**

    If the value of this option is non-`nil`, Emacs resizes windows in units of pixels. This currently affects functions like `split-window` (see [Splitting Windows](Splitting-Windows.html)), `maximize-window`, `minimize-window`, `fit-window-to-buffer`, `fit-frame-to-buffer` and `shrink-window-if-larger-than-buffer` (all listed below).

    Note that when a frame’s pixel size is not a multiple of its character size, at least one window may get resized pixelwise even if this option is `nil`. The default value is `nil`.

The following commands resize windows in more specific ways. When called interactively, they act on the selected window.

*   Command: **fit-window-to-buffer** *\&optional window max-height min-height max-width min-width preserve-size*

    This command adjusts the height or width of `window` to fit the text in it. It returns non-`nil` if it was able to resize `window`, and `nil` otherwise. If `window` is omitted or `nil`, it defaults to the selected window. Otherwise, it should be a live window.

    If `window` is part of a vertical combination, this function adjusts `window`’s height. The new height is calculated from the actual height of the accessible portion of its buffer. The optional argument `max-height`, if non-`nil`, specifies the maximum total height that this function can give `window`. The optional argument `min-height`, if non-`nil`, specifies the minimum total height that it can give, which overrides the variable `window-min-height`. Both `max-height` and `min-height` are specified in lines and include mode and header line and a bottom divider, if any.

    If `window` is part of a horizontal combination and the value of the option `fit-window-to-buffer-horizontally` (see below) is non-`nil`, this function adjusts `window`’s width. The new width of `window` is calculated from the maximum length of its buffer’s lines that follow the current start position of `window`. The optional argument `max-width` specifies a maximum width and defaults to the width of `window`’s frame. The optional argument `min-width` specifies a minimum width and defaults to `window-min-width`. Both `max-width` and `min-width` are specified in columns and include fringes, margins and scrollbars, if any.

    The optional argument `preserve-size`, if non-`nil`, will install a parameter to preserve the size of `window` during future resize operations (see [Preserving Window Sizes](Preserving-Window-Sizes.html)).

    If the option `fit-frame-to-buffer` (see below) is non-`nil`, this function will try to resize the frame of `window` to fit its contents by calling `fit-frame-to-buffer` (see below).

<!---->

*   User Option: **fit-window-to-buffer-horizontally**

    If this is non-`nil`, `fit-window-to-buffer` can resize windows horizontally. If this is `nil` (the default) `fit-window-to-buffer` never resizes windows horizontally. If this is `only`, it can resize windows horizontally only. Any other value means `fit-window-to-buffer` can resize windows in both dimensions.

<!---->

*   User Option: **fit-frame-to-buffer**

    If this option is non-`nil`, `fit-window-to-buffer` can fit a frame to its buffer. A frame is fit if and only if its root window is a live window and this option is non-`nil`. If this is `horizontally`, frames are fit horizontally only. If this is `vertically`, frames are fit vertically only. Any other non-`nil` value means frames can be resized in both dimensions.

If you have a frame that displays only one window, you can fit that frame to its buffer using the command `fit-frame-to-buffer`.

*   Command: **fit-frame-to-buffer** *\&optional frame max-height min-height max-width min-width only*

    This command adjusts the size of `frame` to display the contents of its buffer exactly. `frame` can be any live frame and defaults to the selected one. Fitting is done only if `frame`’s root window is live. The arguments `max-height`, `min-height`, `max-width` and `min-width` specify bounds on the new total size of `frame`’s root window. `min-height` and `min-width` default to the values of `window-min-height` and `window-min-width` respectively.

    If the optional argument `only` is `vertically`, this function may resize the frame vertically only. If `only` is `horizontally`, it may resize the frame horizontally only.

The behavior of `fit-frame-to-buffer` can be controlled with the help of the two options listed next.

*   User Option: **fit-frame-to-buffer-margins**

    This option can be used to specify margins around frames to be fit by `fit-frame-to-buffer`. Such margins can be useful to avoid, for example, that the resized frame overlaps the taskbar or parts of its parent frame.

    It specifies the numbers of pixels to be left free on the left, above, the right, and below a frame that shall be fit. The default specifies `nil` for each which means to use no margins. The value specified here can be overridden for a specific frame by that frame’s `fit-frame-to-buffer-margins` parameter, if present.

<!---->

*   User Option: **fit-frame-to-buffer-sizes**

    This option specifies size boundaries for `fit-frame-to-buffer`. It specifies the total maximum and minimum lines and maximum and minimum columns of the root window of any frame that shall be fit to its buffer. If any of these values is non-`nil`, it overrides the corresponding argument of `fit-frame-to-buffer`.

<!---->

*   Command: **shrink-window-if-larger-than-buffer** *\&optional window*

    This command attempts to reduce `window`’s height as much as possible while still showing its full buffer, but no less than `window-min-height` lines. The return value is non-`nil` if the window was resized, and `nil` otherwise. If `window` is omitted or `nil`, it defaults to the selected window. Otherwise, it should be a live window.

    This command does nothing if the window is already too short to display all of its buffer, or if any of the buffer is scrolled off-screen, or if the window is the only live window in its frame.

    This command calls `fit-window-to-buffer` (see above) to do its work.

<!---->

*   Command: **balance-windows** *\&optional window-or-frame*

    This function balances windows in a way that gives more space to full-width and/or full-height windows. If `window-or-frame` specifies a frame, it balances all windows on that frame. If `window-or-frame` specifies a window, it balances only that window and its siblings (see [Windows and Frames](Windows-and-Frames.html)).

<!---->

*   Command: **balance-windows-area**

    This function attempts to give all windows on the selected frame approximately the same share of the screen area. Full-width or full-height windows are not given more space than other windows.

<!---->

*   Command: **maximize-window** *\&optional window*

    This function attempts to make `window` as large as possible, in both dimensions, without resizing its frame or deleting other windows. If `window` is omitted or `nil`, it defaults to the selected window.

<!---->

*   Command: **minimize-window** *\&optional window*

    This function attempts to make `window` as small as possible, in both dimensions, without deleting it or resizing its frame. If `window` is omitted or `nil`, it defaults to the selected window.

Next: [Preserving Window Sizes](Preserving-Window-Sizes.html), Previous: [Window Sizes](Window-Sizes.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
