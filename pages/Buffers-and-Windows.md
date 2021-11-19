

Next: [Switching Buffers](Switching-Buffers.html), Previous: [Cyclic Window Ordering](Cyclic-Window-Ordering.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.11 Buffers and Windows

This section describes low-level functions for examining and setting the contents of windows. See [Switching Buffers](Switching-Buffers.html), for higher-level functions for displaying a specific buffer in a window.

*   Function: **window-buffer** *\&optional window*

    This function returns the buffer that `window` is displaying. If `window` is omitted or `nil` it defaults to the selected window. If `window` is an internal window, this function returns `nil`.

<!---->

*   Function: **set-window-buffer** *window buffer-or-name \&optional keep-margins*

    This function makes `window` display `buffer-or-name`. `window` should be a live window; if `nil`, it defaults to the selected window. `buffer-or-name` should be a buffer, or the name of an existing buffer. This function does not change which window is selected, nor does it directly change which buffer is current (see [Current Buffer](Current-Buffer.html)). Its return value is `nil`.

    If `window` is *strongly dedicated* to a buffer and `buffer-or-name` does not specify that buffer, this function signals an error. See [Dedicated Windows](Dedicated-Windows.html).

    By default, this function resets `window`’s position, display margins, fringe widths, and scroll bar settings, based on the local variables in the specified buffer. However, if the optional argument `keep-margins` is non-`nil`, it leaves `window`’s display margins, fringes and scroll bar settings alone.

    When writing an application, you should normally use `display-buffer` (see [Choosing Window](Choosing-Window.html)) or the higher-level functions described in [Switching Buffers](Switching-Buffers.html), instead of calling `set-window-buffer` directly.

    This runs `window-scroll-functions`, followed by `window-configuration-change-hook`. See [Window Hooks](Window-Hooks.html).

<!---->

*   Variable: **buffer-display-count**

    This buffer-local variable records the number of times a buffer has been displayed in a window. It is incremented each time `set-window-buffer` is called for the buffer.

<!---->

*   Variable: **buffer-display-time**

    This buffer-local variable records the time at which a buffer was last displayed in a window. The value is `nil` if the buffer has never been displayed. It is updated each time `set-window-buffer` is called for the buffer, with the value returned by `current-time` (see [Time of Day](Time-of-Day.html)).

<!---->

*   Function: **get-buffer-window** *\&optional buffer-or-name all-frames*

    This function returns the first window displaying `buffer-or-name` in the cyclic ordering of windows, starting from the selected window (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)). If no such window exists, the return value is `nil`.

    `buffer-or-name` should be a buffer or the name of a buffer; if omitted or `nil`, it defaults to the current buffer. The optional argument `all-frames` specifies which windows to consider:

    *   `t` means consider windows on all existing frames.
    *   `visible` means consider windows on all visible frames.
    *   0 means consider windows on all visible or iconified frames.
    *   A frame means consider windows on that frame only.
    *   Any other value means consider windows on the selected frame.

    Note that these meanings differ slightly from those of the `all-frames` argument to `next-window` (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)). This function may be changed in a future version of Emacs to eliminate this discrepancy.

<!---->

*   Function: **get-buffer-window-list** *\&optional buffer-or-name minibuf all-frames*

    This function returns a list of all windows currently displaying `buffer-or-name`. `buffer-or-name` should be a buffer or the name of an existing buffer. If omitted or `nil`, it defaults to the current buffer. If the currently selected window displays `buffer-or-name`, it will be the first in the list returned by this function.

    The arguments `minibuf` and `all-frames` have the same meanings as in the function `next-window` (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)). Note that the `all-frames` argument does *not* behave exactly like in `get-buffer-window`.

<!---->

*   Command: **replace-buffer-in-windows** *\&optional buffer-or-name*

    This command replaces `buffer-or-name` with some other buffer, in all windows displaying it. `buffer-or-name` should be a buffer, or the name of an existing buffer; if omitted or `nil`, it defaults to the current buffer.

    The replacement buffer in each window is chosen via `switch-to-prev-buffer` (see [Window History](Window-History.html)). Any dedicated window displaying `buffer-or-name` is deleted if possible (see [Dedicated Windows](Dedicated-Windows.html)). If such a window is the only window on its frame and there are other frames on the same terminal, the frame is deleted as well. If the dedicated window is the only window on the only frame on its terminal, the buffer is replaced anyway.

Next: [Switching Buffers](Switching-Buffers.html), Previous: [Cyclic Window Ordering](Cyclic-Window-Ordering.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
