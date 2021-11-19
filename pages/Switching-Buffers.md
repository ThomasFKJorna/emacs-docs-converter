

Next: [Displaying Buffers](Displaying-Buffers.html), Previous: [Buffers and Windows](Buffers-and-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.12 Switching to a Buffer in a Window

This section describes high-level functions for switching to a specified buffer in some window. In general, “switching to a buffer” means to (1) show the buffer in some window, (2) make that window the selected window (and its frame the selected frame), and (3) make the buffer the current buffer.

Do *not* use these functions to make a buffer temporarily current just so a Lisp program can access or modify it. They have side-effects, such as changing window histories (see [Window History](Window-History.html)), which will surprise the user if used that way. If you want to make a buffer current to modify it in Lisp, use `with-current-buffer`, `save-current-buffer`, or `set-buffer`. See [Current Buffer](Current-Buffer.html).

*   Command: **switch-to-buffer** *buffer-or-name \&optional norecord force-same-window*

    This command attempts to display `buffer-or-name` in the selected window and make it the current buffer. It is often used interactively (as the binding of `C-x b`), as well as in Lisp programs. The return value is the buffer switched to.

    If `buffer-or-name` is `nil`, it defaults to the buffer returned by `other-buffer` (see [Buffer List](Buffer-List.html)). If `buffer-or-name` is a string that is not the name of any existing buffer, this function creates a new buffer with that name; the new buffer’s major mode is determined by the variable `major-mode` (see [Major Modes](Major-Modes.html)).

    Normally, the specified buffer is put at the front of the buffer list—both the global buffer list and the selected frame’s buffer list (see [Buffer List](Buffer-List.html)). However, this is not done if the optional argument `norecord` is non-`nil`.

    Sometimes, the selected window may not be suitable for displaying the buffer. This happens if the selected window is a minibuffer window, or if the selected window is strongly dedicated to its buffer (see [Dedicated Windows](Dedicated-Windows.html)). In such cases, the command normally tries to display the buffer in some other window, by invoking `pop-to-buffer` (see below).

    If the optional argument `force-same-window` is non-`nil` and the selected window is not suitable for displaying the buffer, this function always signals an error when called non-interactively. In interactive use, if the selected window is a minibuffer window, this function will try to use some other window instead. If the selected window is strongly dedicated to its buffer, the option `switch-to-buffer-in-dedicated-window` described next can be used to proceed.

<!---->

*   User Option: **switch-to-buffer-in-dedicated-window**

    This option, if non-`nil`, allows `switch-to-buffer` to proceed when called interactively and the selected window is strongly dedicated to its buffer.

    The following values are respected:

    *   `nil`

        Disallows switching and signals an error as in non-interactive use.

    *   `prompt`

        Prompts the user whether to allow switching.

    *   `pop`

        Invokes `pop-to-buffer` to proceed.

    *   `t`

        Marks the selected window as non-dedicated and proceeds.

    This option does not affect non-interactive calls of `switch-to-buffer`.

By default, `switch-to-buffer` tries to preserve `window-point`. This behavior can be tuned using the following option.

*   User Option: **switch-to-buffer-preserve-window-point**

    If this variable is `nil`, `switch-to-buffer` displays the buffer specified by `buffer-or-name` at the position of that buffer’s `point`. If this variable is `already-displayed`, it tries to display the buffer at its previous position in the selected window, provided the buffer is currently displayed in some other window on any visible or iconified frame. If this variable is `t`, `switch-to-buffer` unconditionally tries to display the buffer at its previous position in the selected window.

    This variable is ignored if the buffer is already displayed in the selected window or never appeared in it before, or if `switch-to-buffer` calls `pop-to-buffer` to display the buffer.

<!---->

*   User Option: **switch-to-buffer-obey-display-actions**

    If this variable is non-`nil`, `switch-to-buffer` respects display actions specified by `display-buffer-overriding-action`, `display-buffer-alist` and other display related variables.

The next two commands are similar to `switch-to-buffer`, except for the described features.

*   Command: **switch-to-buffer-other-window** *buffer-or-name \&optional norecord*

    This function displays the buffer specified by `buffer-or-name` in some window other than the selected window. It uses the function `pop-to-buffer` internally (see below).

    If the selected window already displays the specified buffer, it continues to do so, but another window is nonetheless found to display it as well.

    The `buffer-or-name` and `norecord` arguments have the same meanings as in `switch-to-buffer`.

<!---->

*   Command: **switch-to-buffer-other-frame** *buffer-or-name \&optional norecord*

    This function displays the buffer specified by `buffer-or-name` in a new frame. It uses the function `pop-to-buffer` internally (see below).

    If the specified buffer is already displayed in another window, in any frame on the current terminal, this switches to that window instead of creating a new frame. However, the selected window is never used for this.

    The `buffer-or-name` and `norecord` arguments have the same meanings as in `switch-to-buffer`.

The above commands use the function `pop-to-buffer`, which flexibly displays a buffer in some window and selects that window for editing. In turn, `pop-to-buffer` uses `display-buffer` for displaying the buffer. Hence, all the variables affecting `display-buffer` will affect it as well. See [Choosing Window](Choosing-Window.html), for the documentation of `display-buffer`.

*   Command: **pop-to-buffer** *buffer-or-name \&optional action norecord*

    This function makes `buffer-or-name` the current buffer and displays it in some window, preferably not the window currently selected. It then selects the displaying window. If that window is on a different graphical frame, that frame is given input focus if possible (see [Input Focus](Input-Focus.html)).

    If `buffer-or-name` is `nil`, it defaults to the buffer returned by `other-buffer` (see [Buffer List](Buffer-List.html)). If `buffer-or-name` is a string that is not the name of any existing buffer, this function creates a new buffer with that name; the new buffer’s major mode is determined by the variable `major-mode` (see [Major Modes](Major-Modes.html)). In any case, that buffer is made current and returned, even when no suitable window was found to display it.

    If `action` is non-`nil`, it should be a display action to pass to `display-buffer` (see [Choosing Window](Choosing-Window.html)). Alternatively, a non-`nil`, non-list value means to pop to a window other than the selected one—even if the buffer is already displayed in the selected window.

    Like `switch-to-buffer`, this function updates the buffer list unless `norecord` is non-`nil`.

Next: [Displaying Buffers](Displaying-Buffers.html), Previous: [Buffers and Windows](Buffers-and-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
