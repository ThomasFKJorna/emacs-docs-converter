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

Next: [Splitting Windows](Splitting-Windows.html), Previous: [Resizing Windows](Resizing-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.5 Preserving Window Sizes

A window can get resized explicitly by using one of the functions from the preceding section or implicitly, for example, when resizing an adjacent window, when splitting or deleting a window (see [Splitting Windows](Splitting-Windows.html), see [Deleting Windows](Deleting-Windows.html)) or when resizing the window’s frame (see [Frame Size](Frame-Size.html)).

It is possible to avoid implicit resizing of a specific window when there are one or more other resizable windows on the same frame. For this purpose, Emacs must be advised to *preserve* the size of that window. There are two basic ways to do that.

*   Variable: **window-size-fixed**

    If this buffer-local variable is non-`nil`, the size of any window displaying the buffer cannot normally be changed. Deleting a window or changing the frame’s size may still change the window’s size, if there is no choice.

    If the value is `height`, then only the window’s height is fixed; if the value is `width`, then only the window’s width is fixed. Any other non-`nil` value fixes both the width and the height.

    If this variable is `nil`, this does not necessarily mean that any window showing the buffer can be resized in the desired direction. To determine that, use the function `window-resizable`. See [Resizing Windows](Resizing-Windows.html).

Often `window-size-fixed` is overly aggressive because it inhibits any attempt to explicitly resize or split an affected window as well. This may even happen after the window has been resized implicitly, for example, when deleting an adjacent window or resizing the window’s frame. The following function tries hard to never disallow resizing such a window explicitly:

*   Function: **window-preserve-size** *\&optional window horizontal preserve*

    This function (un-)marks the height of window `window` as preserved for future resize operations. `window` must be a live window and defaults to the selected one. If the optional argument `horizontal` is non-`nil`, it (un-)marks the width of `window` as preserved.

    If the optional argument `preserve` is `t`, this means to preserve the current height/width of `window`’s body. The height/width of `window` will change only if Emacs has no better choice. Resizing a window whose height/width is preserved by this function never throws an error.

    If `preserve` is `nil`, this means to stop preserving the height/width of `window`, lifting any respective restraint induced by a previous call of this function for `window`. Calling `enlarge-window`, `shrink-window` or `fit-window-to-buffer` with `window` as argument may also remove the respective restraint.

`window-preserve-size` is currently invoked by the following functions:

*   `fit-window-to-buffer`

    If the optional argument `preserve-size` of that function (see [Resizing Windows](Resizing-Windows.html)) is non-`nil`, the size established by that function is preserved.

*   `display-buffer`

    If the `alist` argument of that function (see [Choosing Window](Choosing-Window.html)) contains a `preserve-size` entry, the size of the window produced by that function is preserved.

`window-preserve-size` installs a window parameter (see [Window Parameters](Window-Parameters.html)) called `window-preserved-size` which is consulted by the window resizing functions. This parameter will not prevent resizing the window when the window shows another buffer than the one when `window-preserve-size` was invoked or if its size has changed since then.

The following function can be used to check whether the height of a particular window is preserved:

*   Function: **window-preserved-size** *\&optional window horizontal*

    This function returns the preserved height of window `window` in pixels. `window` must be a live window and defaults to the selected one. If the optional argument `horizontal` is non-`nil`, it returns the preserved width of `window`. It returns `nil` if the size of `window` is not preserved.

Next: [Splitting Windows](Splitting-Windows.html), Previous: [Resizing Windows](Resizing-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
