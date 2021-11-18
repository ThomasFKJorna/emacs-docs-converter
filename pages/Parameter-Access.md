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

Next: [Initial Parameters](Initial-Parameters.html), Up: [Frame Parameters](Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 29.4.1 Access to Frame Parameters

These functions let you read and change the parameter values of a frame.

*   Function: **frame-parameter** *frame parameter*

    This function returns the value of the parameter `parameter` (a symbol) of `frame`. If `frame` is `nil`, it returns the selected frame’s parameter. If `frame` has no setting for `parameter`, this function returns `nil`.

<!---->

*   Function: **frame-parameters** *\&optional frame*

    The function `frame-parameters` returns an alist listing all the parameters of `frame` and their values. If `frame` is `nil` or omitted, this returns the selected frame’s parameters

<!---->

*   Function: **modify-frame-parameters** *frame alist*

    This function alters the frame `frame` based on the elements of `alist`. Each element of `alist` has the form `(parm . value)`, where `parm` is a symbol naming a parameter. If you don’t mention a parameter in `alist`, its value doesn’t change. If `frame` is `nil`, it defaults to the selected frame.

    Some parameters are only meaningful for frames on certain kinds of display (see [Frames](Frames.html)). If `alist` includes parameters that are not meaningful for the `frame`’s display, this function will change its value in the frame’s parameter list, but will otherwise ignore it.

    When `alist` specifies more than one parameter whose value can affect the new size of `frame`, the final size of the frame may differ according to the toolkit used. For example, specifying that a frame should from now on have a menu and/or tool bar instead of none and simultaneously specifying the new height of the frame will inevitably lead to a recalculation of the frame’s height. Conceptually, in such case, this function will try to have the explicit height specification prevail. It cannot be excluded, however, that the addition (or removal) of the menu or tool bar, when eventually performed by the toolkit, will defeat this intention.

    Sometimes, binding `frame-inhibit-implied-resize` (see [Implied Frame Resizing](Implied-Frame-Resizing.html)) to a non-`nil` value around calls to this function may fix the problem sketched here. Sometimes, however, exactly such binding may be hit by the problem.

<!---->

*   Function: **set-frame-parameter** *frame parm value*

    This function sets the frame parameter `parm` to the specified `value`. If `frame` is `nil`, it defaults to the selected frame.

<!---->

*   Function: **modify-all-frames-parameters** *alist*

    This function alters the frame parameters of all existing frames according to `alist`, then modifies `default-frame-alist` (and, if necessary, `initial-frame-alist`) to apply the same parameter values to frames that will be created henceforth.

Next: [Initial Parameters](Initial-Parameters.html), Up: [Frame Parameters](Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
