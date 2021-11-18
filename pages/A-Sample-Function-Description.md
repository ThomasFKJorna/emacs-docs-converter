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

Next: [A Sample Variable Description](A-Sample-Variable-Description.html), Up: [Format of Descriptions](Format-of-Descriptions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 1.3.7.1 A Sample Function Description

In a function description, the name of the function being described appears first. It is followed on the same line by a list of argument names. These names are also used in the body of the description, to stand for the values of the arguments.

The appearance of the keyword `&optional` in the argument list indicates that the subsequent arguments may be omitted (omitted arguments default to `nil`). Do not write `&optional` when you call the function.

The keyword `&rest` (which must be followed by a single argument name) indicates that any number of arguments can follow. The single argument name following `&rest` receives, as its value, a list of all the remaining arguments passed to the function. Do not write `&rest` when you call the function.

Here is a description of an imaginary function `foo`:

*   Function: **foo** *integer1 \&optional integer2 \&rest integers*

    The function `foo` subtracts `integer1` from `integer2`, then adds all the rest of the arguments to the result. If `integer2` is not supplied, then the number 19 is used by default.

        (foo 1 5 3 9)
             ⇒ 16
        (foo 5)
             ⇒ 14

    More generally,

        (foo w x y…)
        ≡
        (+ (- x w) y…)

By convention, any argument whose name contains the name of a type (e.g., `integer`, `integer1` or `buffer`) is expected to be of that type. A plural of a type (such as `buffers`) often means a list of objects of that type. An argument named `object` may be of any type. (For a list of Emacs object types, see [Lisp Data Types](Lisp-Data-Types.html).) An argument with any other sort of name (e.g., `new-file`) is specific to the function; if the function has a documentation string, the type of the argument should be described there (see [Documentation](Documentation.html)).

See [Lambda Expressions](Lambda-Expressions.html), for a more complete description of arguments modified by `&optional` and `&rest`.

Command, macro, and special form descriptions have the same format, but the word ‘`Function`’ is replaced by ‘`Command`’, ‘`Macro`’, or ‘`Special Form`’, respectively. Commands are simply functions that may be called interactively; macros process their arguments differently from functions (the arguments are not evaluated), but are presented the same way.

The descriptions of macros and special forms use a more complex notation to specify optional and repeated arguments, because they can break the argument list down into separate arguments in more complicated ways. ‘`[optional-arg]`’ means that `optional-arg` is optional and ‘`repeated-args…`’ stands for zero or more arguments. Parentheses are used when several arguments are grouped into additional levels of list structure. Here is an example:

*   Special Form: **count-loop** *(var \[from to \[inc]]) body…*

    This imaginary special form implements a loop that executes the `body` forms and then increments the variable `var` on each iteration. On the first iteration, the variable has the value `from`; on subsequent iterations, it is incremented by one (or by `inc` if that is given). The loop exits before executing `body` if `var` equals `to`. Here is an example:

        (count-loop (i 0 10)
          (prin1 i) (princ " ")
          (prin1 (aref vector i))
          (terpri))

    If `from` and `to` are omitted, `var` is bound to `nil` before the loop begins, and the loop exits if `var` is non-`nil` at the beginning of an iteration. Here is an example:

        (count-loop (done)
          (if (pending)
              (fixit)
            (setq done t)))

    In this special form, the arguments `from` and `to` are optional, but must both be present or both absent. If they are present, `inc` may optionally be specified as well. These arguments are grouped with the argument `var` into a list, to distinguish them from `body`, which includes all remaining elements of the form.

Next: [A Sample Variable Description](A-Sample-Variable-Description.html), Up: [Format of Descriptions](Format-of-Descriptions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
