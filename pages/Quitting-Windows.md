

Next: [Side Windows](Side-Windows.html), Previous: [Dedicated Windows](Dedicated-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.16 Quitting Windows

When you want to get rid of a window used for displaying a buffer, you can call `delete-window` or `delete-windows-on` (see [Deleting Windows](Deleting-Windows.html)) to remove that window from its frame. If the buffer is shown on a separate frame, you might want to call `delete-frame` (see [Deleting Frames](Deleting-Frames.html)) instead. If, on the other hand, a window has been reused for displaying the buffer, you might prefer showing the buffer previously shown in that window, by calling the function `switch-to-prev-buffer` (see [Window History](Window-History.html)). Finally, you might want to either bury (see [Buffer List](Buffer-List.html)) or kill (see [Killing Buffers](Killing-Buffers.html)) the window’s buffer.

The following command uses information on how the window for displaying the buffer was obtained in the first place, thus attempting to automate the above decisions for you.

*   Command: **quit-window** *\&optional kill window*

    This command quits `window` and buries its buffer. The argument `window` must be a live window and defaults to the selected one. With prefix argument `kill` non-`nil`, it kills the buffer instead of burying it. It calls the function `quit-restore-window` described next to deal with the window and its buffer.

    The functions in `quit-window-hook` are run before doing anything else.

<!---->

*   Function: **quit-restore-window** *\&optional window bury-or-kill*

    This function handles `window` and its buffer after quitting. The optional argument `window` must be a live window and defaults to the selected one. The function’s behavior is determined by the four elements of the list specified by `window`’s `quit-restore` parameter (see [Window Parameters](Window-Parameters.html)).

    The first element of the `quit-restore` parameter is one of the symbols `window`, meaning that the window has been specially created by `display-buffer`; `frame`, a separate frame has been created; `same`, the window has only ever displayed this buffer; or `other`, the window showed another buffer before. `frame` and `window` affect how the window is quit, while `same` and `other` affect the redisplay of buffers previously shown in `window`.

    The parameter’s second element is either one of the symbols `window` or `frame`, or a list whose elements are the buffer shown in `window` before, that buffer’s window start and window point positions, and `window`’s height at that time. If that buffer is still live when `window` is quit, then this function may reuse `window` to display it.

    The third element is the window selected at the time the parameter was created. If this function deletes `window`, it subsequently tries to reselect the window named by that element.

    The fourth element is the buffer whose display caused the creation of this parameter. This function may delete `window` if and only if it still shows that buffer.

    This function will try to delete `window` if and only if (1) the first element of its `quit-restore` parameter is either `window` or `frame`, (2) the window has no history of previously-displayed buffers and (3) the fourth element of the `quit-restore` parameter specifies the buffer currently displayed in `window`. If `window` is part of an atomic window (see [Atomic Windows](Atomic-Windows.html)), it will try to delete the root of that atomic window instead. In either case, it tries to avoid signaling an error when `window` cannot be deleted.

    If `window` shall be deleted, is the only window on its frame and there are other frames on that frame’s terminal, the value of the optional argument `bury-or-kill` determines how to proceed with the window. If `bury-or-kill` equals `kill`, the frame is deleted unconditionally. Otherwise, the fate of the frame is determined by calling `frame-auto-hide-function` (see below) with that frame as sole argument.

    If the third element of the `quit-restore` parameter is a list of buffer, window start (see [Window Start and End](Window-Start-and-End.html)), and point (see [Window Point](Window-Point.html)), and that buffer is still live, the buffer will be displayed, and start and point set accordingly. If, in addition, `window`’s buffer was temporarily resized, this function will also try to restore the original height of `window`.

    Otherwise, if `window` was previously used for displaying other buffers (see [Window History](Window-History.html)), the most recent buffer in that history will be displayed. In either case, if `window` is not deleted, its `quit-restore` parameter is reset to `nil`.

    The optional argument `bury-or-kill` specifies how to deal with `window`’s buffer. The following values are handled:

    *   `nil`

        This means to not deal with the buffer in any particular way. As a consequence, if `window` is not deleted, invoking `switch-to-prev-buffer` will usually show the buffer again.

    *   `append`

        This means that if `window` is not deleted, its buffer is moved to the end of `window`’s list of previous buffers, so it’s less likely that a future invocation of `switch-to-prev-buffer` will switch to it. Also, it moves the buffer to the end of the frame’s buffer list.

    *   `bury`

        This means that if `window` is not deleted, its buffer is removed from `window`’s list of previous buffers. Also, it moves the buffer to the end of the frame’s buffer list. This value provides the most reliable remedy to not have `switch-to-prev-buffer` switch to this buffer again without killing the buffer.

    *   `kill`

        This means to kill `window`’s buffer.

    Typically, the display routines run by `display-buffer` will set the `quit-restore` window parameter correctly. It’s also possible to set it manually, using the following code for displaying `buffer` in `window`:

    ```lisp
    (display-buffer-record-window type window buffer)

    (set-window-buffer window buffer)

    (set-window-prev-buffers window nil)
    ```

    Setting the window history to `nil` ensures that a future call to `quit-window` can delete the window altogether.

The following option specifies how to deal with a frame containing just one window that should be either quit, or whose buffer should be buried.

*   User Option: **frame-auto-hide-function**

    The function specified by this option is called to automatically hide frames. This function is called with one argument—a frame.

    The function specified here is called by `bury-buffer` (see [Buffer List](Buffer-List.html)) when the selected window is dedicated and shows the buffer to bury. It is also called by `quit-restore-window` (see above) when the frame of the window to quit has been specially created for displaying that window’s buffer and the buffer is not killed.

    The default is to call `iconify-frame` (see [Visibility of Frames](Visibility-of-Frames.html)). Alternatively, you may specify either `delete-frame` (see [Deleting Frames](Deleting-Frames.html)) to remove the frame from its display, `make-frame-invisible` to make the frame invisible, `ignore` to leave the frame unchanged, or any other function that can take a frame as its sole argument.

    Note that the function specified by this option is called only if the specified frame contains just one live window and there is at least one other frame on the same terminal.

    For a particular frame, the value specified here may be overridden by that frame’s `auto-hide-function` frame parameter (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)).

Next: [Side Windows](Side-Windows.html), Previous: [Dedicated Windows](Dedicated-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
