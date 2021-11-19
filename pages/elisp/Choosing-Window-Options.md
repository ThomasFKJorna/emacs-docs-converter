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

Next: [Precedence of Action Functions](Precedence-of-Action-Functions.html), Previous: [Buffer Display Action Alists](Buffer-Display-Action-Alists.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 28.13.4 Additional Options for Displaying Buffers

The behavior of buffer display actions (see [Choosing Window](Choosing-Window.html)) can be further modified by the following user options.

*   User Option: **pop-up-windows**

    If the value of this variable is non-`nil`, `display-buffer` is allowed to split an existing window to make a new window for displaying in. This is the default.

    This variable is provided for backward compatibility only. It is obeyed by `display-buffer` via a special mechanism in `display-buffer-fallback-action`, which calls the action function `display-buffer-pop-up-window` (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)) when the value of this option is non-`nil`. It is not consulted by `display-buffer-pop-up-window` itself, which the user may specify directly in `display-buffer-alist` etc.

<!---->

*   User Option: **split-window-preferred-function**

    This variable specifies a function for splitting a window, in order to make a new window for displaying a buffer. It is used by the `display-buffer-pop-up-window` action function to actually split the window.

    The value must be a function that takes one argument, a window, and returns either a new window (which will be used to display the desired buffer) or `nil` (which means the splitting failed). The default value is `split-window-sensibly`, which is documented next.

<!---->

*   Function: **split-window-sensibly** *\&optional window*

    This function tries to split `window` and return the newly created window. If `window` cannot be split, it returns `nil`. If `window` is omitted or `nil`, it defaults to the selected window.

    This function obeys the usual rules that determine when a window may be split (see [Splitting Windows](Splitting-Windows.html)). It first tries to split by placing the new window below, subject to the restriction imposed by `split-height-threshold` (see below), in addition to any other restrictions. If that fails, it tries to split by placing the new window to the right, subject to `split-width-threshold` (see below). If that also fails, and the window is the only window on its frame, this function again tries to split and place the new window below, disregarding `split-height-threshold`. If this fails as well, this function gives up and returns `nil`.

<!---->

*   User Option: **split-height-threshold**

    This variable specifies whether `split-window-sensibly` is allowed to split the window placing the new window below. If it is an integer, that means to split only if the original window has at least that many lines. If it is `nil`, that means not to split this way.

<!---->

*   User Option: **split-width-threshold**

    This variable specifies whether `split-window-sensibly` is allowed to split the window placing the new window to the right. If the value is an integer, that means to split only if the original window has at least that many columns. If the value is `nil`, that means not to split this way.

<!---->

*   User Option: **even-window-sizes**

    This variable, if non-`nil`, causes `display-buffer` to even window sizes whenever it reuses an existing window, and that window is adjacent to the selected one.

    If its value is `width-only`, sizes are evened only if the reused window is on the left or right of the selected one and the selected window is wider than the reused one. If its value is `height-only` sizes are evened only if the reused window is above or beneath the selected window and the selected window is higher than the reused one. Any other non-`nil` value means to even sizes in any of these cases provided the selected window is larger than the reused one in the sense of their combination.

<!---->

*   User Option: **pop-up-frames**

    If the value of this variable is non-`nil`, that means `display-buffer` may display buffers by making new frames. The default is `nil`.

    A non-`nil` value also means that when `display-buffer` is looking for a window already displaying `buffer-or-name`, it can search any visible or iconified frame, not just the selected frame.

    This variable is provided mainly for backward compatibility. It is obeyed by `display-buffer` via a special mechanism in `display-buffer-fallback-action`, which calls the action function `display-buffer-pop-up-frame` (see [Buffer Display Action Functions](Buffer-Display-Action-Functions.html)) if the value is non-`nil`. (This is done before attempting to split a window.) This variable is not consulted by `display-buffer-pop-up-frame` itself, which the user may specify directly in `display-buffer-alist` etc.

<!---->

*   User Option: **pop-up-frame-function**

    This variable specifies a function for creating a new frame, in order to make a new window for displaying a buffer. It is used by the `display-buffer-pop-up-frame` action function.

    The value should be a function that takes no arguments and returns a frame, or `nil` if no frame could be created. The default value is a function that creates a frame using the parameters specified by `pop-up-frame-alist` (see below).

<!---->

*   User Option: **pop-up-frame-alist**

    This variable holds an alist of frame parameters (see [Frame Parameters](Frame-Parameters.html)), which is used by the function specified by `pop-up-frame-function` to make a new frame. The default is `nil`.

    This option is provided for backward compatibility only. Note, that when `display-buffer-pop-up-frame` calls the function specified by `pop-up-frame-function`, it prepends the value of all `pop-up-frame-parameters` action alist entries to `pop-up-frame-alist` so that the values specified by the action alist entry effectively override any corresponding values of `pop-up-frame-alist`.

    Hence, users should set up a `pop-up-frame-parameters` action alist entry in `display-buffer-alist` instead of customizing `pop-up-frame-alist`. Only this will guarantee that the value of a parameter specified by the user overrides the value of that parameter specified by the caller of `display-buffer`.

Many efforts in the design of `display-buffer` have been given to maintain compatibility with code that uses older options like `pop-up-windows`, `pop-up-frames`, `pop-up-frame-alist`, `same-window-buffer-names` and `same-window-regexps`. Lisp Programs and users should refrain from using these options. Above we already warned against customizing `pop-up-frame-alist`. Here we describe how to convert the remaining options to use display actions instead.

*   `pop-up-windows`

    This variable is `t` by default. Instead of customizing it to `nil` and thus telling `display-buffer` what not to do, it’s much better to list in `display-buffer-base-action` the action functions it should try instead as, for example:

        (customize-set-variable
         'display-buffer-base-action
         '((display-buffer-reuse-window display-buffer-same-window
            display-buffer-in-previous-window
            display-buffer-use-some-window)))

*   `pop-up-frames`

    Instead of customizing this variable to `t`, customize `display-buffer-base-action`, for example, as follows:

        (customize-set-variable
         'display-buffer-base-action
         '((display-buffer-reuse-window display-buffer-pop-up-frame)
           (reusable-frames . 0)))

*   *   `same-window-buffer-names`
    *   `same-window-regexps`

    Instead of adding a buffer name or a regular expression to one of these options use a `display-buffer-alist` entry for that buffer specifying the action function `display-buffer-same-window`.

        (customize-set-variable
         'display-buffer-alist
         (cons '("\\*foo\\*" (display-buffer-same-window))
                display-buffer-alist))

Next: [Precedence of Action Functions](Precedence-of-Action-Functions.html), Previous: [Buffer Display Action Alists](Buffer-Display-Action-Alists.html), Up: [Displaying Buffers](Displaying-Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
