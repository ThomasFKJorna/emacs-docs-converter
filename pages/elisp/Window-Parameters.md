

### 28.27 Window Parameters

This section describes the window parameters that can be used to associate additional information with windows.

### Function: **window-parameter** *window parameter*

This function returns `window`’s value for `parameter`. The default for `window` is the selected window. If `window` has no setting for `parameter`, this function returns `nil`.

### Function: **window-parameters** *\&optional window*

This function returns all parameters of `window` and their values. The default for `window` is the selected window. The return value is either `nil`, or an association list whose elements have the form `(parameter . value)`.

### Function: **set-window-parameter** *window parameter value*

This function sets `window`’s value of `parameter` to `value` and returns `value`. The default for `window` is the selected window.

By default, the functions that save and restore window configurations or the states of windows (see [Window Configurations](Window-Configurations.html)) do not care about window parameters. This means that when you change the value of a parameter within the body of a `save-window-excursion`, the previous value is not restored when that macro exits. It also means that when you restore via `window-state-put` a window state saved earlier by `window-state-get`, all cloned windows have their parameters reset to `nil`. The following variable allows you to override the standard behavior:

### Variable: **window-persistent-parameters**

This variable is an alist specifying which parameters get saved by `current-window-configuration` and `window-state-get`, and subsequently restored by `set-window-configuration` and `window-state-put`. See [Window Configurations](Window-Configurations.html).

The CAR of each entry of this alist is a symbol specifying the parameter. The CDR should be one of the following:

*   `nil`

    This value means the parameter is saved neither by `window-state-get` nor by `current-window-configuration`.

*   `t`

    This value specifies that the parameter is saved by `current-window-configuration` and (provided its `writable` argument is `nil`) by `window-state-get`.

*   `writable`

    This means that the parameter is saved unconditionally by both `current-window-configuration` and `window-state-get`. This value should not be used for parameters whose values do not have a read syntax. Otherwise, invoking `window-state-put` in another session may fail with an `invalid-read-syntax` error.

Some functions (notably `delete-window`, `delete-other-windows` and `split-window`), may behave specially when the window specified by their `window` argument has a parameter whose name is equal to the function’s name. You can override such special behavior by binding the following variable to a non-`nil` value:

### Variable: **ignore-window-parameters**

If this variable is non-`nil`, some standard functions do not process window parameters. The functions currently affected by this are `split-window`, `delete-window`, `delete-other-windows`, and `other-window`.

An application can bind this variable to a non-`nil` value around calls to these functions. If it does so, the application is fully responsible for correctly assigning the parameters of all involved windows when exiting that function.

The following parameters are currently used by the window management code:

`delete-window`

This parameter affects the execution of `delete-window` (see [Deleting Windows](Deleting-Windows.html)).

`delete-other-windows`

This parameter affects the execution of `delete-other-windows` (see [Deleting Windows](Deleting-Windows.html)).

`no-delete-other-windows`

This parameter marks the window as not deletable by `delete-other-windows` (see [Deleting Windows](Deleting-Windows.html)).

`split-window`

This parameter affects the execution of `split-window` (see [Splitting Windows](Splitting-Windows.html)).

`other-window`

This parameter affects the execution of `other-window` (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)).

`no-other-window`

This parameter marks the window as not selectable by `other-window` (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)).

`clone-of`

This parameter specifies the window that this one has been cloned from. It is installed by `window-state-get` (see [Window Configurations](Window-Configurations.html)).

`window-preserved-size`

This parameter specifies a buffer, a direction where `nil` means vertical and `t` horizontal, and a size in pixels. If this window displays the specified buffer and its size in the indicated direction equals the size specified by this parameter, then Emacs will try to preserve the size of this window in the indicated direction. This parameter is installed and updated by the function `window-preserve-size` (see [Preserving Window Sizes](Preserving-Window-Sizes.html)).

`quit-restore`

This parameter is installed by the buffer display functions (see [Choosing Window](Choosing-Window.html)) and consulted by `quit-restore-window` (see [Quitting Windows](Quitting-Windows.html)). It is a list of four elements, see the description of `quit-restore-window` in [Quitting Windows](Quitting-Windows.html) for details.

*   `window-side`
*   `window-slot`

These parameters are used internally for implementing side windows (see [Side Windows](Side-Windows.html)).

`window-atom`

This parameter is used internally for implementing atomic windows, see [Atomic Windows](Atomic-Windows.html).

`mode-line-format`

This parameter replaces the value of the buffer-local variable `mode-line-format` (see [Mode Line Basics](Mode-Line-Basics.html)) of this window’s buffer whenever this window is displayed. The symbol `none` means to suppress display of a mode line for this window. Display and contents of the mode line on other windows showing this buffer are not affected.

`header-line-format`

This parameter replaces the value of the buffer-local variable `header-line-format` (see [Mode Line Basics](Mode-Line-Basics.html)) of this window’s buffer whenever this window is displayed. The symbol `none` means to suppress display of a header line for this window. Display and contents of the header line on other windows showing this buffer are not affected.

`tab-line-format`

This parameter replaces the value of the buffer-local variable `tab-line-format` (see [Mode Line Basics](Mode-Line-Basics.html)) of this window’s buffer whenever this window is displayed. The symbol `none` means to suppress display of a tab line for this window. Display and contents of the tab line on other windows showing this buffer are not affected.

`min-margins`

The value of this parameter is a cons cell whose CAR and CDR, if non-`nil`, specify the minimum values (in columns) for the left and right margin of this window (see [Display Margins](Display-Margins.html). When present, Emacs will use these values instead of the actual margin widths for determining whether a window can be split or shrunk horizontally.

Emacs never auto-adjusts the margins of any window after splitting or resizing it. It is the sole responsibility of any application setting this parameter to adjust the margins of this window as well as those of any new window that inherits this window’s margins due to a split. Both `window-configuration-change-hook` and `window-size-change-functions` (see [Window Hooks](Window-Hooks.html)) should be employed for this purpose.

This parameter was introduced in Emacs version 25.1 to support applications that use large margins to center buffer text within a window and should be used, with due care, exclusively by those applications. It might be replaced by an improved solution in future versions of Emacs.
