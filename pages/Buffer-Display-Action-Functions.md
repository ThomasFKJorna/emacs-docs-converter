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

Next: [Buffer Display Action Alists](Buffer-Display-Action-Alists.html), Previous: [Choosing Window](Choosing-Window.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 28.13.2 Action Functions for Buffer Display

An *action function* is a function `display-buffer` calls for choosing a window to display a buffer. Action functions take two arguments: `buffer`, the buffer to display, and `alist`, an action alist (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)). They are supposed to return a window displaying `buffer` if they succeed and `nil` if they fail.

The following basic action functions are defined in Emacs.

*   Function: **display-buffer-same-window** *buffer alist*

    This function tries to display `buffer` in the selected window. It fails if the selected window is a minibuffer window or is dedicated to another buffer (see [Dedicated Windows](Dedicated-Windows.html)). It also fails if `alist` has a non-`nil` `inhibit-same-window` entry.

<!---->

*   Function: **display-buffer-reuse-window** *buffer alist*

    This function tries to display `buffer` by finding a window that is already displaying it. Windows on the selected frame are preferred to windows on other frames.

    If `alist` has a non-`nil` `inhibit-same-window` entry, the selected window is not eligible for reuse. The set of frames to search for a window already displaying `buffer` can be specified with the help of the `reusable-frames` action alist entry. If `alist` contains no `reusable-frames` entry, this function searches just the selected frame.

    If this function chooses a window on another frame, it makes that frame visible and, unless `alist` contains an `inhibit-switch-frame` entry, raises that frame if necessary.

<!---->

*   Function: **display-buffer-reuse-mode-window** *buffer alist*

    This function tries to display `buffer` by finding a window that is displaying a buffer in a given mode.

    If `alist` contains a `mode` entry, its value specifes a major mode (a symbol) or a list of major modes. If `alist` contains no `mode` entry, the current major mode of `buffer` is used instead. A window is a candidate if it displays a buffer whose mode derives from one of the modes specified thusly.

    The behavior is also controlled by `alist` entries for `inhibit-same-window`, `reusable-frames` and `inhibit-switch-frame`, like `display-buffer-reuse-window` does.

<!---->

*   Function: **display-buffer-pop-up-window** *buffer alist*

    This function tries to display `buffer` by splitting the largest or least recently-used window (usually located on the selected frame). It actually performs the split by calling the function specified by `split-window-preferred-function` (see [Choosing Window Options](Choosing-Window-Options.html)).

    The size of the new window can be adjusted by supplying `window-height` and `window-width` entries in `alist`. If `alist` contains a `preserve-size` entry, Emacs will also try to preserve the size of the new window during future resize operations (see [Preserving Window Sizes](Preserving-Window-Sizes.html)).

    This function fails if no window can be split. More often than not, this happens because no window is large enough to allow splitting. Setting `split-height-threshold` or `split-width-threshold` to lower values may help in this regard. Splitting also fails when the selected frame has an `unsplittable` frame parameter; see [Buffer Parameters](Buffer-Parameters.html).

<!---->

*   Function: **display-buffer-in-previous-window** *buffer alist*

    This function tries to display `buffer` in a window where it was displayed previously.

    If `alist` contains a non-`nil` `inhibit-same-window` entry, the selected window is not eligible for use. A dedicated window is usable only if it already shows `buffer`. If `alist` contains a `previous-window` entry, the window specified by that entry is usable even if it never showed `buffer` before.

    If `alist` contains a `reusable-frames` entry (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)), its value determines which frames to search for a suitable window. If `alist` contains no `reusable-frames` entry, this function searches just the selected frame if `display-buffer-reuse-frames` and `pop-up-frames` are both `nil`; it searches all frames on the current terminal if either of those variables is non-`nil`.

    If more than one window qualifies as usable according to these rules, this function makes a choice in the following order of preference:

    *   The window specified by any `previous-window` `alist` entry, provided it is not the selected window.
    *   A window that showed `buffer` before, provided it is not the selected window.
    *   The selected window if it is either specified by a `previous-window` `alist` entry or showed `buffer` before.

<!---->

*   Function: **display-buffer-use-some-window** *buffer alist*

    This function tries to display `buffer` by choosing an existing window and displaying the buffer in that window. It can fail if all windows are dedicated to other buffers (see [Dedicated Windows](Dedicated-Windows.html)).

<!---->

*   Function: **display-buffer-in-direction** *buffer alist*

    This function tries to display `buffer` at a location specified by `alist`. For this purpose, `alist` should contain a `direction` entry whose value is one of `left`, `above` (or `up`), `right` and `below` (or `down`). Other values are usually interpreted as `below`.

    If `alist` also contains a `window` entry, its value specifies a reference window. That value can be a special symbol like `main` which stands for the selected frame’s main window (see [Side Window Options and Functions](Side-Window-Options-and-Functions.html)) or `root` standing for the selected frame’s root window (see [Windows and Frames](Windows-and-Frames.html)). It can also specify an arbitrary valid window. Any other value (or omitting the `window` entry entirely) means to use the selected window as reference window.

    This function first tries to reuse a window in the specified direction that already shows `buffer`. If no such window exists, it tries to split the reference window in order to produce a new window in the specified direction. If this fails as well, it will try to display `buffer` in an existing window in the specified direction. In either case, the window chosen will appear on the side of the reference window specified by the `direction` entry, sharing at least one edge with the reference window.

    If the reference window is live, the edge the chosen window will share with it is always the opposite of the one specified by the `direction` entry. For example, if the value of the `direction` entry is `left`, the chosen window’s right edge coordinate (see [Coordinates and Windows](Coordinates-and-Windows.html)) will equal the reference window’s left edge coordinate.

    If the reference window is internal, a reused window must share with it the edge specified by the `direction` entry. Hence if, for example, the reference window is the frame’s root window and the value of the `direction` entry is `left`, a reused window must be on the left of the frame. This means that the left edge coordinate of the chosen window and that of the reference window are the same.

    A new window, however, will be created by splitting the reference window such that the chosen window will share the opposite edge with the reference window. In our example, a new root window would be created with a new live window and the reference window as its children. The chosen window’s right edge coordinate would then equal the left edge coordinate of the reference window. Its left edge coordinate would equal the left edge coordinate of the frame’s new root window.

    Four special values for `direction` entries allow to implicitly specify the selected frame’s main window as the reference window: `leftmost`, `top`, `rightmost` and `bottom`. This means that instead of, for example, `(direction . left) (window . main)`<!-- /@w --> one can just specify `(direction . leftmost)`<!-- /@w -->. An existing `window` `alist` entry is ignored in such cases.

<!---->

*   Function: **display-buffer-below-selected** *buffer alist*

    This function tries to display `buffer` in a window below the selected window. If there is a window below the selected one and that window already displays `buffer`, it reuses that window.

    If there is no such window, this function tries to create a new window by splitting the selected one, and displays `buffer` there. It will also try to adjust that window’s size provided `alist` contains a suitable `window-height` or `window-width` entry, see above.

    If splitting the selected window fails and there is a non-dedicated window below the selected one showing some other buffer, this function tries to use that window for showing `buffer`.

    If `alist` contains a `window-min-height` entry, this function ensures that the window used is or can become at least as high as specified by that entry’s value. Note that this is only a guarantee. In order to actually resize the window used, `alist` must also provide an appropriate `window-height` entry.

<!---->

*   Function: **display-buffer-at-bottom** *buffer alist*

    This function tries to display `buffer` in a window at the bottom of the selected frame.

    This either tries to split the window at the bottom of the frame or the frame’s root window, or to reuse an existing window at the bottom of the selected frame.

<!---->

*   Function: **display-buffer-pop-up-frame** *buffer alist*

    This function creates a new frame, and displays the buffer in that frame’s window. It actually performs the frame creation by calling the function specified in `pop-up-frame-function` (see [Choosing Window Options](Choosing-Window-Options.html)). If `alist` contains a `pop-up-frame-parameters` entry, the associated value is added to the newly created frame’s parameters.

<!---->

*   Function: **display-buffer-in-child-frame** *buffer alist*

    This function tries to display `buffer` in a child frame (see [Child Frames](Child-Frames.html)) of the selected frame, either reusing an existing child frame or by making a new one. If `alist` has a non-`nil` `child-frame-parameters` entry, the corresponding value is an alist of frame parameters to give the new frame. A `parent-frame` parameter specifying the selected frame is provided by default. If the child frame should become the child of another frame, a corresponding entry must be added to `alist`.

    The appearance of child frames is largely dependent on the parameters provided via `alist`. It is advisable to use at least ratios to specify the size (see [Size Parameters](Size-Parameters.html)) and the position (see [Position Parameters](Position-Parameters.html)) of the child frame, and to add a `keep-ratio` parameter (see [Frame Interaction Parameters](Frame-Interaction-Parameters.html)), in order to make sure that the child frame remains visible. For other parameters that should be considered see [Child Frames](Child-Frames.html).

<!---->

*   Function: **display-buffer-use-some-frame** *buffer alist*

    This function tries to display `buffer` by finding a frame that meets a predicate (by default any frame other than the selected frame).

    If this function chooses a window on another frame, it makes that frame visible and, unless `alist` contains an `inhibit-switch-frame` entry, raises that frame if necessary.

    If `alist` has a non-`nil` `frame-predicate` entry, its value is a function taking one argument (a frame), returning non-`nil` if the frame is a candidate; this function replaces the default predicate.

    If `alist` has a non-`nil` `inhibit-same-window` entry, the selected window is not used; thus if the selected frame has a single window, it is not used.

<!---->

*   Function: **display-buffer-no-window** *buffer alist*

    If `alist` has a non-`nil` `allow-no-window` entry, then this function does not display `buffer` and returns the symbol `fail`. This constitutes the only exception to the convention that an action function returns either `nil` or a window showing `buffer`. If `alist` has no such `allow-no-window` entry, this function returns `nil`.

    If this function returns `fail`, `display-buffer` will skip the execution of any further display actions and return `nil` immediately. If this function returns `nil`, `display-buffer` will continue with the next display action, if any.

    It is assumed that when a caller of `display-buffer` specifies a non-`nil` `allow-no-window` entry, it is also able to handle a `nil` return value.

Two other action functions are described in their proper sections—`display-buffer-in-side-window` (see [Displaying Buffers in Side Windows](Displaying-Buffers-in-Side-Windows.html)) and `display-buffer-in-atom-window` (see [Atomic Windows](Atomic-Windows.html)).

Next: [Buffer Display Action Alists](Buffer-Display-Action-Alists.html), Previous: [Choosing Window](Choosing-Window.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
