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

Next: [Warning Variables](Warning-Variables.html), Up: [Warnings](Warnings.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.5.1 Warning Basics

Every warning has a textual message, which explains the problem for the user, and a *severity level* which is a symbol. Here are the possible severity levels, in order of decreasing severity, and their meanings:

*   `:emergency`

    A problem that will seriously impair Emacs operation soon if you do not attend to it promptly.

*   `:error`

    A report of data or circumstances that are inherently wrong.

*   `:warning`

    A report of data or circumstances that are not inherently wrong, but raise suspicion of a possible problem.

*   `:debug`

    A report of information that may be useful if you are debugging.

When your program encounters invalid input data, it can either signal a Lisp error by calling `error` or `signal` or report a warning with severity `:error`. Signaling a Lisp error is the easiest thing to do, but it means the program cannot continue processing. If you want to take the trouble to implement a way to continue processing despite the bad data, then reporting a warning of severity `:error` is the right way to inform the user of the problem. For instance, the Emacs Lisp byte compiler can report an error that way and continue compiling other functions. (If the program signals a Lisp error and then handles it with `condition-case`, the user won’t see the error message; it could show the message to the user by reporting it as a warning.)

Each warning has a *warning type* to classify it. The type is a list of symbols. The first symbol should be the custom group that you use for the program’s user options. For example, byte compiler warnings use the warning type `(bytecomp)`. You can also subcategorize the warnings, if you wish, by using more symbols in the list.

*   Function: **display-warning** *type message \&optional level buffer-name*

    This function reports a warning, using `message` as the message and `type` as the warning type. `level` should be the severity level, with `:warning` being the default.

    `buffer-name`, if non-`nil`, specifies the name of the buffer for logging the warning. By default, it is `*Warnings*`.

<!---->

*   Function: **lwarn** *type level message \&rest args*

    This function reports a warning using the value of `(format-message message args...)` as the message in the `*Warnings*` buffer. In other respects it is equivalent to `display-warning`.

<!---->

*   Function: **warn** *message \&rest args*

    This function reports a warning using the value of `(format-message message args...)` as the message, `(emacs)` as the type, and `:warning` as the severity level. It exists for compatibility only; we recommend not using it, because you should specify a specific warning type.

Next: [Warning Variables](Warning-Variables.html), Up: [Warnings](Warnings.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
