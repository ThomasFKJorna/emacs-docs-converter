

### 28.14 Window History

Each window remembers in a list the buffers it has previously displayed, and the order in which these buffers were removed from it. This history is used, for example, by `replace-buffer-in-windows` (see [Buffers and Windows](Buffers-and-Windows.html)), and when quitting windows (see [Quitting Windows](Quitting-Windows.html)). The list is automatically maintained by Emacs, but you can use the following functions to explicitly inspect or alter it:

### Function: **window-prev-buffers** *\&optional window*

This function returns a list specifying the previous contents of `window`. The optional argument `window` should be a live window and defaults to the selected one.

Each list element has the form `(buffer window-start window-pos)`, where `buffer` is a buffer previously shown in the window, `window-start` is the window start position (see [Window Start and End](Window-Start-and-End.html)) when that buffer was last shown, and `window-pos` is the point position (see [Window Point](Window-Point.html)) when that buffer was last shown in `window`.

The list is ordered so that earlier elements correspond to more recently-shown buffers, and the first element usually corresponds to the buffer most recently removed from the window.

### Function: **set-window-prev-buffers** *window prev-buffers*

This function sets `window`’s previous buffers to the value of `prev-buffers`. The argument `window` must be a live window and defaults to the selected one. The argument `prev-buffers` should be a list of the same form as that returned by `window-prev-buffers`.

In addition, each window maintains a list of *next buffers*, which is a list of buffers re-shown by `switch-to-prev-buffer` (see below). This list is mainly used by `switch-to-prev-buffer` and `switch-to-next-buffer` for choosing buffers to switch to.

### Function: **window-next-buffers** *\&optional window*

This function returns the list of buffers recently re-shown in `window` via `switch-to-prev-buffer`. The `window` argument must denote a live window or `nil` (meaning the selected window).

### Function: **set-window-next-buffers** *window next-buffers*

This function sets the next buffer list of `window` to `next-buffers`. The `window` argument should be a live window or `nil` (meaning the selected window). The argument `next-buffers` should be a list of buffers.

The following commands can be used to cycle through the global buffer list, much like `bury-buffer` and `unbury-buffer`. However, they cycle according to the specified window’s history list, rather than the global buffer list. In addition, they restore window-specific window start and point positions, and may show a buffer even if it is already shown in another window. The `switch-to-prev-buffer` command, in particular, is used by `replace-buffer-in-windows`, `bury-buffer` and `quit-window` to find a replacement buffer for a window.

### Command: **switch-to-prev-buffer** *\&optional window bury-or-kill*

This command displays the previous buffer in `window`. The argument `window` should be a live window or `nil` (meaning the selected window). If the optional argument `bury-or-kill` is non-`nil`, this means that the buffer currently shown in `window` is about to be buried or killed and consequently should not be switched to in future invocations of this command.

The previous buffer is usually the buffer shown before the buffer currently shown in `window`. However, a buffer that has been buried or killed, or has been already shown by a recent invocation of `switch-to-prev-buffer`, does not qualify as previous buffer.

If repeated invocations of this command have already shown all buffers previously shown in `window`, further invocations will show buffers from the buffer list of the frame `window` appears on (see [Buffer List](Buffer-List.html)).

The option `switch-to-prev-buffer-skip` described below can be used to inhibit switching to certain buffers, for example, to those already shown in another window. Also, if `window`’s frame has a `buffer-predicate` parameter (see [Buffer Parameters](Buffer-Parameters.html)), that predicate may inhibit switching to certain buffers.

### Command: **switch-to-next-buffer** *\&optional window*

This command switches to the next buffer in `window`, thus undoing the effect of the last `switch-to-prev-buffer` command in `window`. The argument `window` must be a live window and defaults to the selected one.

If there is no recent invocation of `switch-to-prev-buffer` that can be undone, this function tries to show a buffer from the buffer list of the frame `window` appears on (see [Buffer List](Buffer-List.html)).

The option `switch-to-prev-buffer-skip` and the `buffer-predicate` (see [Buffer Parameters](Buffer-Parameters.html)) of `window`’s frame affect this command as they do for `switch-to-prev-buffer`.

By default `switch-to-prev-buffer` and `switch-to-next-buffer` can switch to a buffer that is already shown in another window. The following option can be used to override this behavior.

### User Option: **switch-to-prev-buffer-skip**

If this variable is `nil`, `switch-to-prev-buffer` may switch to any buffer, including those already shown in other windows.

If this variable is non-`nil`, `switch-to-prev-buffer` will refrain from switching to certain buffers. The following values can be used:

*   `this` means do not switch to a buffer shown on the frame that hosts the window `switch-to-prev-buffer` is acting upon.
*   `visible` means do not switch to a buffer shown on any visible frame.
*   0 (the number zero) means do not switch to a buffer shown on any visible or iconified frame.
*   `t` means do not switch to a buffer shown on any live frame.
*   A function that takes three arguments—the `window` argument of `switch-to-prev-buffer`, a buffer `switch-to-prev-buffer` intends to switch to and the `bury-or-kill` argument of `switch-to-prev-buffer`. If that function returns non-`nil`, `switch-to-prev-buffer` will refrain from switching to the buffer specified by the second argument.

The command `switch-to-next-buffer` obeys this option in a similar way. If this option specifies a function, `switch-to-next-buffer` will call that function with the third argument always `nil`.

Note that since `switch-to-prev-buffer` is called by `bury-buffer`, `replace-buffer-in-windows` and `quit-restore-window` as well, customizing this option may also affect the behavior of Emacs when a window is quit or a buffer gets buried or killed.

Note also that under certain circumstances `switch-to-prev-buffer` and `switch-to-next-buffer` may ignore this option, for example, when there is only one buffer left these functions can switch to.
