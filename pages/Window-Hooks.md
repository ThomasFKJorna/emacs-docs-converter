<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Previous: [Window Parameters](Window-Parameters.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.28 Hooks for Window Scrolling and Changes

This section describes how Lisp programs can take action after a window has been scrolled or other window modifications occurred. We first consider the case where a window shows a different part of its buffer.

*   Variable: **window-scroll-functions**

    This variable holds a list of functions that Emacs should call before redisplaying a window with scrolling. Displaying a different buffer in a window and making a new window also call these functions.

    This variable is not a normal hook, because each function is called with two arguments: the window, and its new display-start position. At the time of the call, the display-start position of the argument window is already set to its new value, and the buffer to be displayed in the window is set as the current buffer.

    These functions must take care when using `window-end` (see [Window Start and End](Window-Start-and-End.html)); if you need an up-to-date value, you must use the `update` argument to ensure you get it.

    **Warning:** don’t use this feature to alter the way the window is scrolled. It’s not designed for that, and such use probably won’t work.

In addition, you can use `jit-lock-register` to register a Font Lock fontification function, which will be called whenever parts of a buffer are (re)fontified because a window was scrolled or its size changed. See [Other Font Lock Variables](Other-Font-Lock-Variables.html).

The remainder of this section covers six hooks that are called during redisplay provided a significant, non-scrolling change of a window has been detected. For simplicity, these hooks and the functions they call will be collectively referred to as *window change functions*.

The first of these hooks is run after a *window buffer change* is detected, which means that a window was created, deleted or assigned another buffer.

*   Variable: **window-buffer-change-functions**

    This variable specifies functions called during redisplay when window buffers have changed. The value should be a list of functions that take one argument.

    Functions specified buffer-locally are called for any window showing the corresponding buffer if that window has been created or assigned that buffer since the last time window change functions were run. In this case the window is passed as argument.

    Functions specified by the default value are called for a frame if at least one window on that frame has been added, deleted or assigned another buffer since the last time window change functions were run. In this case the frame is passed as argument.

The second of these hooks is run when a *window size change* has been detected which means that a window was created, assigned another buffer, or changed its total size or that of its text area.

*   Variable: **window-size-change-functions**

    This variable specifies functions called during redisplay when a window size change occurred. The value should be a list of functions that take one argument.

    Functions specified buffer-locally are called for any window showing the corresponding buffer if that window has been added or assigned another buffer or changed its total or body size since the last time window change functions were run. In this case the window is passed as argument.

    Functions specified by the default value are called for a frame if at least one window on that frame has been added or assigned another buffer or changed its total or body size since the last time window change functions were run. In this case the frame is passed as argument.

The third of these hooks is run when a *window selection change* has selected another window since the last redisplay.

*   Variable: **window-selection-change-functions**

    This variable specifies functions called during redisplay when the selected window or a frame’s selected window has changed. The value should be a list of functions that take one argument.

    Functions specified buffer-locally are called for any window showing the corresponding buffer if that window has been selected or deselected (among all windows or among all windows on its frame) since the last time window change functions were run. In this case the window is passed as argument.

    Functions specified by the default value are called for a frame if that frame has been selected or deselected or the frame’s selected window has changed since the last time window change functions were run. In this case the frame is passed as argument.

The fourth of these hooks is run when a *window state change* has been detected, which means that at least one of the three preceding window changes has occurred.

*   Variable: **window-state-change-functions**

    This variable specifies functions called during redisplay when a window buffer or size change occurred or the selected window or a frame’s selected window has changed. The value should be a list of functions that take one argument.

    Functions specified buffer-locally are called for any window showing the corresponding buffer if that window has been added or assigned another buffer, changed its total or body size or has been selected or deselected (among all windows or among all windows on its frame) since the last time window change functions were run. In this case the window is passed as argument.

    Functions specified by the default value are called for a frame if at least one window on that frame has been added, deleted or assigned another buffer, changed its total or body size or that frame has been selected or deselected or the frame’s selected window has changed since the last time window change functions were run. In this case the frame is passed as argument.

    Functions specified by the default value are also run for a frame when that frame’s window state change flag (see below) has been set since last redisplay.

The fifth of these hooks is run when a *window configuration change* has been detected which means that either the buffer or the size of a window changed. It differs from the four preceding hooks in the way it is run.

*   Variable: **window-configuration-change-hook**

    This variable specifies functions called during redisplay when either the buffer or the size of a window has changed. The value should be a list of functions that take no argument.

    Functions specified buffer-locally are called for any window showing the corresponding buffer if at least one window on that frame has been added, deleted or assigned another buffer or changed its total or body size since the last time window change functions were run. Each call is performed with the window showing the buffer temporarily selected and its buffer current.

    Functions specified by the default value are called for each frame if at least one window on that frame has been added, deleted or assigned another buffer or changed its total or body size since the last time window change functions were run. Each call is performed with the frame temporarily selected and the selected window’s buffer current.

Finally, Emacs runs a normal hook that generalizes the behavior of `window-state-change-functions`.

*   Variable: **window-state-change-hook**

    The default value of this variable specifies functions called during redisplay when a window state change has been detected or the window state change flag has been set on at least one frame. The value should be a list of functions that take no argument.

    Applications should put a function on this hook only if they want to react to changes that happened on (or have been signaled for) two or more frames since last redisplay. In every other case, putting the function on `window-state-change-functions` should be preferred.

Window change functions are called during redisplay for each frame as follows: First, any buffer-local window buffer change function, window size change function, selected window change and window state change functions are called in this order. Next, the default values for these functions are called in the same order. Then any buffer-local window configuration change functions are called followed by functions specified by the default value of those functions. Finally, functions on `window-state-change-hook` are run.

Window change functions are run for a specific frame only if a corresponding change was registered for that frame earlier. Such changes include the creation or deletion of a window or the assignment of another buffer or size to a window. Note that even when such a change has been registered, this does not mean that any of the hooks described above is run. If, for example, a change was registered within the scope of a window excursion (see [Window Configurations](Window-Configurations.html)), this will trigger a call of window change functions only if that excursion still persists at the time change functions are run. If it is exited earlier, hooks will be run only if registered by a change outside the scope of that excursion.

The *window state change flag* of a frame, if set, will cause the default values of `window-state-change-functions` (for that frame) and `window-state-change-hook` to be run during next redisplay regardless of whether a window state change actually occurred for that frame or not. After running any functions on these hooks, the flag is reset for each frame. Applications can set that flag and inspect its value using the following functions.

*   Function: **set-frame-window-state-change** *\&optional frame arg*

    This function sets `frame`’s window state change flag if `arg` is non-`nil` and resets it otherwise. `frame` must be a live frame and defaults to the selected one.

<!---->

*   Function: **frame-window-state-change** *\&optional frame*

    This functions returns `t` if `frame`’s window state change flag is set and `nil` otherwise. `frame` must be a live frame and defaults to the selected one.

While window change functions are run, the functions described next can be called to get more insight into what has changed for a specific window or frame since the last redisplay. All these functions take a live window as single, optional argument, defaulting to the selected window.

*   Function: **window-old-buffer** *\&optional window*

    This function returns the buffer shown in `window` at the last time window change functions were run for `window`’s frame. If it returns `nil`, `window` has been created after that. If it returns `t`, `window` was not shown at that time but has been restored from a previously saved window configuration afterwards. Otherwise, the return value is the buffer shown by `window` at that time.

<!---->

*   Function: **window-old-pixel-width** *\&optional window*

    This function returns the total pixel width of `window` the last time window change functions found `window` live on its frame. It is zero if `window` was created after that.

<!---->

*   Function: **window-old-pixel-height** *\&optional window*

    This function returns the total pixel height of `window` the last time window change functions found `window` live on its frame. It is zero if `window` was created after that.

<!---->

*   Function: **window-old-body-pixel-width** *\&optional window*

    This function returns the pixel width of `window`’s text area the last time window change functions found `window` live on its frame. It is zero if `window` was created after that.

<!---->

*   Function: **window-old-body-pixel-height** *\&optional window*

    This function returns the pixel height of `window`’s text area the last time window change functions found `window` live on its frame. It is zero if `window` was created after that.

In order to find out which window or frame was selected the last time window change functions were run, the following functions can be used:

*   Function: **frame-old-selected-window** *\&optional frame*

    This function returns the selected window of `frame` at the last time window change functions were run. If omitted or `nil` `frame` defaults to the selected frame.

<!---->

*   Function: **old-selected-window**

    This function returns the selected window at the last time window change functions were run.

<!---->

*   Function: **old-selected-frame**

    This function returns the selected frame at the last time window change functions were run.

Note that window change functions provide no information about which windows have been deleted since the last time they were run. If necessary, applications should remember any window showing a specific buffer in a local variable of that buffer and update it in a function run by the default values of any of the hooks that are run when a window buffer change was detected.

The following caveats should be considered when adding a function to window change functions:

*   Some operations will not trigger a call of window change functions. These include showing another buffer in a minibuffer window or any change of a tooltip window.
*   Window change functions should not create or delete windows or change the buffer, size or selection status of any window because there is no guarantee that the information about such a change will be propagated to other window change functions. If at all, any such change should be executed only by the last function listed by the default value of `window-state-change-hook`.
*   Macros like `save-window-excursion`, `with-selected-window` or `with-current-buffer` can be used when running window change functions.
*   Running window change functions does not save and restore match data. Unless running `window-configuration-change-hook` it does not save or restore the selected window or frame or the current buffer either.
*   Any redisplay triggering the run of window change functions may be aborted. If the abort occurs before window change functions have run to their completion, they will be run again with the previous values, that is, as if redisplay had not been performed. If aborted later, they will be run with the new values, that is, as if redisplay had been actually performed.

Previous: [Window Parameters](Window-Parameters.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
