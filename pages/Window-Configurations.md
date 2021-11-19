

Next: [Window Parameters](Window-Parameters.html), Previous: [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.26 Window Configurations

A *window configuration* records the entire layout of one frame—all windows, their sizes, which buffers they contain, how those buffers are scrolled, and their value of point; also their fringes, margins, and scroll bar settings. It also includes the value of `minibuffer-scroll-window`. As a special exception, the window configuration does not record the value of point in the selected window for the current buffer.

You can bring back an entire frame layout by restoring a previously saved window configuration. If you want to record the layout of all frames instead of just one, use a frame configuration instead of a window configuration. See [Frame Configurations](Frame-Configurations.html).

*   Function: **current-window-configuration** *\&optional frame*

    This function returns a new object representing `frame`’s current window configuration. The default for `frame` is the selected frame. The variable `window-persistent-parameters` specifies which window parameters (if any) are saved by this function. See [Window Parameters](Window-Parameters.html).

<!---->

*   Function: **set-window-configuration** *configuration*

    This function restores the configuration of windows and buffers as specified by `configuration`, for the frame that `configuration` was created for, regardless of whether that frame is selected or not. The argument `configuration` must be a value that was previously returned by `current-window-configuration` for that frame.

    If the frame from which `configuration` was saved is dead, all this function does is to restore the value of the variable `minibuffer-scroll-window` and to adjust the value returned by `minibuffer-selected-window`. In this case, the function returns `nil`. Otherwise, it returns `t`.

    If the buffer of a window of `configuration` has been killed since `configuration` was made, that window is, as a rule, removed from the restored configuration. However, if that window is the last window remaining in the restored configuration, another live buffer is shown in it.

    Here is a way of using this function to get the same effect as `save-window-excursion`:

    ```lisp
    (let ((config (current-window-configuration)))
      (unwind-protect
          (progn (split-window-below nil)
                 …)
        (set-window-configuration config)))
    ```

<!---->

*   Macro: **save-window-excursion** *forms…*

    This macro records the window configuration of the selected frame, executes `forms` in sequence, then restores the earlier window configuration. The return value is the value of the final form in `forms`.

    Most Lisp code should not use this macro; `save-selected-window` is typically sufficient. In particular, this macro cannot reliably prevent the code in `forms` from opening new windows, because new windows might be opened in other frames (see [Choosing Window](Choosing-Window.html)), and `save-window-excursion` only saves and restores the window configuration on the current frame.

<!---->

*   Function: **window-configuration-p** *object*

    This function returns `t` if `object` is a window configuration.

<!---->

*   Function: **compare-window-configurations** *config1 config2*

    This function compares two window configurations as regards the structure of windows, but ignores the values of point and the saved scrolling positions—it can return `t` even if those aspects differ.

    The function `equal` can also compare two window configurations; it regards configurations as unequal if they differ in any respect, even a saved point.

<!---->

*   Function: **window-configuration-frame** *config*

    This function returns the frame for which the window configuration `config` was made.

Other primitives to look inside of window configurations would make sense, but are not implemented because we did not need them. See the file `winner.el` for some more operations on windows configurations.

The objects returned by `current-window-configuration` die together with the Emacs process. In order to store a window configuration on disk and read it back in another Emacs session, you can use the functions described next. These functions are also useful to clone the state of a frame into an arbitrary live window (`set-window-configuration` effectively clones the windows of a frame into the root window of that very frame only).

*   Function: **window-state-get** *\&optional window writable*

    This function returns the state of `window` as a Lisp object. The argument `window` must be a valid window and defaults to the root window of the selected frame.

    If the optional argument `writable` is non-`nil`, this means to not use markers for sampling positions like `window-point` or `window-start`. This argument should be non-`nil` when the state will be written to disk and read back in another session.

    Together, the argument `writable` and the variable `window-persistent-parameters` specify which window parameters are saved by this function. See [Window Parameters](Window-Parameters.html).

The value returned by `window-state-get` can be used in the same session to make a clone of a window in another window. It can be also written to disk and read back in another session. In either case, use the following function to restore the state of the window.

*   Function: **window-state-put** *state \&optional window ignore*

    This function puts the window state `state` into `window`. The argument `state` should be the state of a window returned by an earlier invocation of `window-state-get`, see above. The optional argument `window` can be either a live window or an internal window (see [Windows and Frames](Windows-and-Frames.html)). If `window` is not a live window, it is replaced by a new live window created on the same frame before putting `state` into it. If `window` is `nil`, it puts the window state into a new window.

    If the optional argument `ignore` is non-`nil`, it means to ignore minimum window sizes and fixed-size restrictions. If `ignore` is `safe`, this means windows can get as small as one line and/or two columns.

The functions `window-state-get` and `window-state-put` also allow to exchange the contents of two live windows. The following function does precisely that:

*   Command: **window-swap-states** *\&optional window-1 window-2 size*

    This command swaps the states of the two live windows `window-1` and `window-2`. `window-1` must specify a live window and defaults to the selected one. `window-2` must specify a live window and defaults to the window following `window-1` in the cyclic ordering of windows, excluding minibuffer windows and including live windows on all visible frames.

    Optional argument `size` non-`nil` means to try swapping the sizes of `window-1` and `window-2` as well. A value of `height` means to swap heights only, a value of `width` means to swap widths only, while `t` means to swap both widths and heights, if possible. Frames are not resized by this function.

Next: [Window Parameters](Window-Parameters.html), Previous: [Mouse Window Auto-selection](Mouse-Window-Auto_002dselection.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
