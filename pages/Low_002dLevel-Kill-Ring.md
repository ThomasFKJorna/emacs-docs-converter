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

Next: [Internals of Kill Ring](Internals-of-Kill-Ring.html), Previous: [Yank Commands](Yank-Commands.html), Up: [The Kill Ring](The-Kill-Ring.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 32.8.5 Low-Level Kill Ring

These functions and variables provide access to the kill ring at a lower level, but are still convenient for use in Lisp programs, because they take care of interaction with window system selections (see [Window System Selections](Window-System-Selections.html)).

*   Function: **current-kill** *n \&optional do-not-move*

    The function `current-kill` rotates the yanking pointer, which designates the front of the kill ring, by `n` places (from newer kills to older ones), and returns the text at that place in the ring.

    If the optional second argument `do-not-move` is non-`nil`, then `current-kill` doesn’t alter the yanking pointer; it just returns the `n`th kill, counting from the current yanking pointer.

    If `n` is zero, indicating a request for the latest kill, `current-kill` calls the value of `interprogram-paste-function` (documented below) before consulting the kill ring. If that value is a function and calling it returns a string or a list of several strings, `current-kill` pushes the strings onto the kill ring and returns the first string. It also sets the yanking pointer to point to the kill-ring entry of the first string returned by `interprogram-paste-function`, regardless of the value of `do-not-move`. Otherwise, `current-kill` does not treat a zero value for `n` specially: it returns the entry pointed at by the yanking pointer and does not move the yanking pointer.

<!---->

*   Function: **kill-new** *string \&optional replace*

    This function pushes the text `string` onto the kill ring and makes the yanking pointer point to it. It discards the oldest entry if appropriate. It also invokes the values of `interprogram-paste-function` (subject to the user option `save-interprogram-paste-before-kill`) and `interprogram-cut-function` (see below).

    If `replace` is non-`nil`, then `kill-new` replaces the first element of the kill ring with `string`, rather than pushing `string` onto the kill ring.

<!---->

*   Function: **kill-append** *string before-p*

    This function appends the text `string` to the first entry in the kill ring and makes the yanking pointer point to the combined entry. Normally `string` goes at the end of the entry, but if `before-p` is non-`nil`, it goes at the beginning. This function calls `kill-new` as a subroutine, thus causing the values of `interprogram-cut-function` and possibly `interprogram-paste-function` (see below) to be invoked by extension.

<!---->

*   Variable: **interprogram-paste-function**

    This variable provides a way of transferring killed text from other programs, when you are using a window system. Its value should be `nil` or a function of no arguments.

    If the value is a function, `current-kill` calls it to get the most recent kill. If the function returns a non-`nil` value, then that value is used as the most recent kill. If it returns `nil`, then the front of the kill ring is used.

    To facilitate support for window systems that support multiple selections, this function may also return a list of strings. In that case, the first string is used as the most recent kill, and all the other strings are pushed onto the kill ring, for easy access by `yank-pop`.

    The normal use of this function is to get the window system’s clipboard as the most recent kill, even if the selection belongs to another application. See [Window System Selections](Window-System-Selections.html). However, if the clipboard contents come from the current Emacs session, this function should return `nil`.

<!---->

*   Variable: **interprogram-cut-function**

    This variable provides a way of communicating killed text to other programs, when you are using a window system. Its value should be `nil` or a function of one required argument.

    If the value is a function, `kill-new` and `kill-append` call it with the new first element of the kill ring as the argument.

    The normal use of this function is to put newly killed text in the window system’s clipboard. See [Window System Selections](Window-System-Selections.html).

Next: [Internals of Kill Ring](Internals-of-Kill-Ring.html), Previous: [Yank Commands](Yank-Commands.html), Up: [The Kill Ring](The-Kill-Ring.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
