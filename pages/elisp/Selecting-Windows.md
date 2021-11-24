

### 28.9 Selecting Windows

### Function: **select-window** *window \&optional norecord*

This function makes `window` the selected window and the window selected within its frame (see [Basic Windows](Basic-Windows.html)), and selects that frame. It also makes `window`’s buffer (see [Buffers and Windows](Buffers-and-Windows.html)) current and sets that buffer’s value of `point` to the value of `window-point` (see [Window Point](Window-Point.html)) in `window`. `window` must be a live window. The return value is `window`.

By default, this function also moves `window`’s buffer to the front of the buffer list (see [Buffer List](Buffer-List.html)) and makes `window` the most recently selected window. If the optional argument `norecord` is non-`nil`, these additional actions are omitted.

In addition, this function by default also tells the display engine to update the display of `window` when its frame gets redisplayed the next time. If `norecord` is non-`nil`, such updates are usually not performed. If, however, `norecord` equals the special symbol `mark-for-redisplay`, the additional actions mentioned above are omitted but `window` will be nevertheless updated.

Note that sometimes selecting a window is not enough to show it, or make its frame the top-most frame on display: you may also need to raise the frame or make sure input focus is directed to that frame. See [Input Focus](Input-Focus.html).

For historical reasons, Emacs does not run a separate hook whenever a window gets selected. Applications and internal routines often temporarily select a window to perform a few actions on it. They do that either to simplify coding—because many functions by default operate on the selected window when no `window` argument is specified—or because some functions did not (and still do not) take a window as argument and always operate(d) on the selected window instead. Running a hook every time a window gets selected for a short time and once more when the previously selected window gets restored is not useful.

However, when its `norecord` argument is `nil`, `select-window` updates the buffer list and thus indirectly runs the normal hook `buffer-list-update-hook` (see [Buffer List](Buffer-List.html)). Consequently, that hook provides one way to run a function whenever a window gets selected more “permanently”.

Since `buffer-list-update-hook` is also run by functions that are not related to window management, it will usually make sense to save the value of the selected window somewhere and compare it with the value of `selected-window` while running that hook. Also, to avoid false positives when using `buffer-list-update-hook`, it is good practice that every `select-window` call supposed to select a window only temporarily passes a non-`nil` `norecord` argument. If possible, the macro `with-selected-window` (see below) should be used in such cases.

Emacs also runs the hook `window-selection-change-functions` whenever the redisplay routine detects that another window has been selected since last redisplay. See [Window Hooks](Window-Hooks.html), for a detailed explanation. `window-state-change-functions` (described in the same section) is another abnormal hook run after a different window has been selected but is triggered by other window changes as well.

The sequence of calls to `select-window` with a non-`nil` `norecord` argument determines an ordering of windows by their selection time. The function `get-lru-window` can be used to retrieve the least recently selected live window (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)).

### Macro: **save-selected-window** *forms…*

This macro records the selected frame, as well as the selected window of each frame, executes `forms` in sequence, then restores the earlier selected frame and windows. It also saves and restores the current buffer. It returns the value of the last form in `forms`.

This macro does not save or restore anything about the sizes, arrangement or contents of windows; therefore, if `forms` change them, the change persists. If the previously selected window of some frame is no longer live at the time of exit from `forms`, that frame’s selected window is left alone. If the previously selected window is no longer live, then whatever window is selected at the end of `forms` remains selected. The current buffer is restored if and only if it is still live when exiting `forms`.

This macro changes neither the ordering of recently selected windows nor the buffer list.

### Macro: **with-selected-window** *window forms…*

This macro selects `window`, executes `forms` in sequence, then restores the previously selected window and current buffer. The ordering of recently selected windows and the buffer list remain unchanged unless you deliberately change them within `forms`; for example, by calling `select-window` with argument `norecord` `nil`. Hence, this macro is the preferred way to temporarily work with `window` as the selected window without needlessly running `buffer-list-update-hook`.

### Function: **frame-selected-window** *\&optional frame*

This function returns the window on `frame` that is selected within that frame. `frame` should be a live frame; if omitted or `nil`, it defaults to the selected frame.

### Function: **set-frame-selected-window** *frame window \&optional norecord*

This function makes `window` the window selected within the frame `frame`. `frame` should be a live frame; if `nil`, it defaults to the selected frame. `window` should be a live window; if `nil`, it defaults to the selected window.

If `frame` is the selected frame, this makes `window` the selected window.

If the optional argument `norecord` is non-`nil`, this function does not alter the list of most recently selected windows, nor the buffer list.

### Function: **window-use-time** *\&optional window*

This functions returns the use time of window `window`. `window` must be a live window and defaults to the selected one.

The *use time* of a window is not really a time value, but an integer that does increase monotonically with each call of `select-window` with a `nil` `norecord` argument. The window with the lowest use time is usually called the least recently used window while the window with the highest use time is called the most recently used one (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)).
