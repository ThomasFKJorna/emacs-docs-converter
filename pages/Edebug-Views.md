

Next: [Edebug Eval](Edebug-Eval.html), Previous: [Trapping Errors](Trapping-Errors.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.8 Edebug Views

These Edebug commands let you view aspects of the buffer and window status as they were before entry to Edebug. The outside window configuration is the collection of windows and contents that were in effect outside of Edebug.

*   *   `P`
    *   `v`

    Switch to viewing the outside window configuration (`edebug-view-outside`). Type `C-x X w` to return to Edebug.

*   `p`

    Temporarily display the outside current buffer with point at its outside position (`edebug-bounce-point`), pausing for one second before returning to Edebug. With a prefix argument `n`, pause for `n` seconds instead.

*   `w`

    Move point back to the current stop point in the source code buffer (`edebug-where`).

    If you use this command in a different window displaying the same buffer, that window will be used instead to display the current definition in the future.

*   `W`

    Toggle whether Edebug saves and restores the outside window configuration (`edebug-toggle-save-windows`).

    With a prefix argument, `W` only toggles saving and restoring of the selected window. To specify a window that is not displaying the source code buffer, you must use `C-x X W` from the global keymap.

You can view the outside window configuration with `v` or just bounce to the point in the current buffer with `p`, even if it is not normally displayed.

After moving point, you may wish to jump back to the stop point. You can do that with `w` from a source code buffer. You can jump back to the stop point in the source code buffer from any buffer using `C-x X w`.

Each time you use `W` to turn saving *off*, Edebug forgets the saved outside window configuration—so that even if you turn saving back *on*, the current window configuration remains unchanged when you next exit Edebug (by continuing the program). However, the automatic redisplay of `*edebug*` and `*edebug-trace*` may conflict with the buffers you wish to see unless you have enough windows open.

Next: [Edebug Eval](Edebug-Eval.html), Previous: [Trapping Errors](Trapping-Errors.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
