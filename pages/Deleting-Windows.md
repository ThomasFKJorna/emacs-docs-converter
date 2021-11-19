

Next: [Recombining Windows](Recombining-Windows.html), Previous: [Splitting Windows](Splitting-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.7 Deleting Windows

*Deleting* a window removes it from the frame’s window tree. If the window is a live window, it disappears from the screen. If the window is an internal window, its child windows are deleted too.

Even after a window is deleted, it continues to exist as a Lisp object, until there are no more references to it. Window deletion can be reversed, by restoring a saved window configuration (see [Window Configurations](Window-Configurations.html)).

*   Command: **delete-window** *\&optional window*

    This function removes `window` from display and returns `nil`. If `window` is omitted or `nil`, it defaults to the selected window.

    If deleting the window would leave no more windows in the window tree (e.g., if it is the only live window in the frame) or all remaining windows on `window`’s frame are side windows (see [Side Windows](Side-Windows.html)), an error is signaled. If `window` is part of an atomic window (see [Atomic Windows](Atomic-Windows.html)), this function tries to delete the root of that atomic window instead.

    By default, the space taken up by `window` is given to one of its adjacent sibling windows, if any. However, if the variable `window-combination-resize` is non-`nil`, the space is proportionally distributed among any remaining windows in the same window combination. See [Recombining Windows](Recombining-Windows.html).

    The behavior of this function may be altered by the window parameters of `window`, so long as the variable `ignore-window-parameters` is `nil`. If the value of the `delete-window` window parameter is `t`, this function ignores all other window parameters. Otherwise, if the value of the `delete-window` window parameter is a function, that function is called with the argument `window`, in lieu of the usual action of `delete-window`. See [Window Parameters](Window-Parameters.html).

<!---->

*   Command: **delete-other-windows** *\&optional window*

    This function makes `window` fill its frame, deleting other windows as necessary. If `window` is omitted or `nil`, it defaults to the selected window. An error is signaled if `window` is a side window (see [Side Windows](Side-Windows.html)). If `window` is part of an atomic window (see [Atomic Windows](Atomic-Windows.html)), this function tries to make the root of that atomic window fill its frame. The return value is `nil`.

    The behavior of this function may be altered by the window parameters of `window`, so long as the variable `ignore-window-parameters` is `nil`. If the value of the `delete-other-windows` window parameter is `t`, this function ignores all other window parameters. Otherwise, if the value of the `delete-other-windows` window parameter is a function, that function is called with the argument `window`, in lieu of the usual action of `delete-other-windows`. See [Window Parameters](Window-Parameters.html).

    Also, if `ignore-window-parameters` is `nil`, this function does not delete any window whose `no-delete-other-windows` parameter is non-`nil`.

<!---->

*   Command: **delete-windows-on** *\&optional buffer-or-name frame*

    This function deletes all windows showing `buffer-or-name`, by calling `delete-window` on those windows. `buffer-or-name` should be a buffer, or the name of a buffer; if omitted or `nil`, it defaults to the current buffer. If there are no windows showing the specified buffer, this function does nothing. If the specified buffer is a minibuffer, an error is signaled.

    If there is a dedicated window showing the buffer, and that window is the only one on its frame, this function also deletes that frame if it is not the only frame on the terminal.

    The optional argument `frame` specifies which frames to operate on:

    *   `nil` means operate on all frames.
    *   `t` means operate on the selected frame.
    *   `visible` means operate on all visible frames.
    *   `0` means operate on all visible or iconified frames.
    *   A frame means operate on that frame.

    Note that this argument does not have the same meaning as in other functions which scan all live windows (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)). Specifically, the meanings of `t` and `nil` here are the opposite of what they are in those other functions.

Next: [Recombining Windows](Recombining-Windows.html), Previous: [Splitting Windows](Splitting-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
