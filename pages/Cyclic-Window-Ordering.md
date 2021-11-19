

Next: [Buffers and Windows](Buffers-and-Windows.html), Previous: [Selecting Windows](Selecting-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.10 Cyclic Ordering of Windows

When you use the command `C-x o` (`other-window`) to select some other window, it moves through live windows in a specific order. For any given configuration of windows, this order never varies. It is called the *cyclic ordering of windows*.

The ordering is determined by a depth-first traversal of each frame’s window tree, retrieving the live windows which are the leaf nodes of the tree (see [Windows and Frames](Windows-and-Frames.html)). If the minibuffer is active, the minibuffer window is included too. The ordering is cyclic, so the last window in the sequence is followed by the first one.

*   Function: **next-window** *\&optional window minibuf all-frames*

    This function returns a live window, the one following `window` in the cyclic ordering of windows. `window` should be a live window; if omitted or `nil`, it defaults to the selected window.

    The optional argument `minibuf` specifies whether minibuffer windows should be included in the cyclic ordering. Normally, when `minibuf` is `nil`, a minibuffer window is included only if it is currently active; this matches the behavior of `C-x o`. (Note that a minibuffer window is active as long as its minibuffer is in use; see [Minibuffers](Minibuffers.html)).

    If `minibuf` is `t`, the cyclic ordering includes all minibuffer windows. If `minibuf` is neither `t` nor `nil`, minibuffer windows are not included even if they are active.

    The optional argument `all-frames` specifies which frames to consider:

    *   `nil` means to consider windows on `window`’s frame. If the minibuffer window is considered (as specified by the `minibuf` argument), then frames that share the minibuffer window are considered too.
    *   `t` means to consider windows on all existing frames.
    *   `visible` means to consider windows on all visible frames.
    *   0 means to consider windows on all visible or iconified frames.
    *   A frame means to consider windows on that specific frame.
    *   Anything else means to consider windows on `window`’s frame, and no others.

    If more than one frame is considered, the cyclic ordering is obtained by appending the orderings for those frames, in the same order as the list of all live frames (see [Finding All Frames](Finding-All-Frames.html)).

<!---->

*   Function: **previous-window** *\&optional window minibuf all-frames*

    This function returns a live window, the one preceding `window` in the cyclic ordering of windows. The other arguments are handled like in `next-window`.

<!---->

*   Command: **other-window** *count \&optional all-frames*

    This function selects a live window, one `count` places from the selected window in the cyclic ordering of windows. If `count` is a positive number, it skips `count` windows forwards; if `count` is negative, it skips -`count` windows backwards; if `count` is zero, that simply re-selects the selected window. When called interactively, `count` is the numeric prefix argument.

    The optional argument `all-frames` has the same meaning as in `next-window`, like a `nil` `minibuf` argument to `next-window`.

    This function does not select a window that has a non-`nil` `no-other-window` window parameter (see [Window Parameters](Window-Parameters.html)), provided that `ignore-window-parameters` is `nil`.

    If the `other-window` parameter of the selected window is a function, and `ignore-window-parameters` is `nil`, that function will be called with the arguments `count` and `all-frames` instead of the normal operation of this function.

<!---->

*   Function: **walk-windows** *fun \&optional minibuf all-frames*

    This function calls the function `fun` once for each live window, with the window as the argument.

    It follows the cyclic ordering of windows. The optional arguments `minibuf` and `all-frames` specify the set of windows included; these have the same arguments as in `next-window`. If `all-frames` specifies a frame, the first window walked is the first window on that frame (the one returned by `frame-first-window`), not necessarily the selected window.

    If `fun` changes the window configuration by splitting or deleting windows, that does not alter the set of windows walked, which is determined prior to calling `fun` for the first time.

<!---->

*   Function: **one-window-p** *\&optional no-mini all-frames*

    This function returns `t` if the selected window is the only live window, and `nil` otherwise.

    If the minibuffer window is active, it is normally considered (so that this function returns `nil`). However, if the optional argument `no-mini` is non-`nil`, the minibuffer window is ignored even if active. The optional argument `all-frames` has the same meaning as for `next-window`.

The following functions return a window which satisfies some criterion, without selecting it:

*   Function: **get-lru-window** *\&optional all-frames dedicated not-selected*

    This function returns a live window which is heuristically the least recently used. The optional argument `all-frames` has the same meaning as in `next-window`.

    If any full-width windows are present, only those windows are considered. A minibuffer window is never a candidate. A dedicated window (see [Dedicated Windows](Dedicated-Windows.html)) is never a candidate unless the optional argument `dedicated` is non-`nil`. The selected window is never returned, unless it is the only candidate. However, if the optional argument `not-selected` is non-`nil`, this function returns `nil` in that case.

<!---->

*   Function: **get-mru-window** *\&optional all-frames dedicated not-selected*

    This function is like `get-lru-window`, but it returns the most recently used window instead. The meaning of the arguments is the same as described for `get-lru-window`.

<!---->

*   Function: **get-largest-window** *\&optional all-frames dedicated not-selected*

    This function returns the window with the largest area (height times width). The optional argument `all-frames` specifies the windows to search, and has the same meaning as in `next-window`.

    A minibuffer window is never a candidate. A dedicated window (see [Dedicated Windows](Dedicated-Windows.html)) is never a candidate unless the optional argument `dedicated` is non-`nil`. The selected window is not a candidate if the optional argument `not-selected` is non-`nil`. If the optional argument `not-selected` is non-`nil` and the selected window is the only candidate, this function returns `nil`.

    If there are two candidate windows of the same size, this function prefers the one that comes first in the cyclic ordering of windows, starting from the selected window.

<!---->

*   Function: **get-window-with-predicate** *predicate \&optional minibuf all-frames default*

    This function calls the function `predicate` for each of the windows in the cyclic order of windows in turn, passing it the window as an argument. If the predicate returns non-`nil` for any window, this function stops and returns that window. If no such window is found, the return value is `default` (which defaults to `nil`).

    The optional arguments `minibuf` and `all-frames` specify the windows to search, and have the same meanings as in `next-window`.

Next: [Buffers and Windows](Buffers-and-Windows.html), Previous: [Selecting Windows](Selecting-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
