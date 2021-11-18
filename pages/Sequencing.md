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

Next: [Conditionals](Conditionals.html), Up: [Control Structures](Control-Structures.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 11.1 Sequencing

Evaluating forms in the order they appear is the most common way control passes from one form to another. In some contexts, such as in a function body, this happens automatically. Elsewhere you must use a control structure construct to do this: `progn`, the simplest control construct of Lisp.

A `progn` special form looks like this:

    (progn a b c …)

and it says to execute the forms `a`, `b`, `c`, and so on, in that order. These forms are called the *body* of the `progn` form. The value of the last form in the body becomes the value of the entire `progn`. `(progn)` returns `nil`.

In the early days of Lisp, `progn` was the only way to execute two or more forms in succession and use the value of the last of them. But programmers found they often needed to use a `progn` in the body of a function, where (at that time) only one form was allowed. So the body of a function was made into an implicit `progn`: several forms are allowed just as in the body of an actual `progn`. Many other control structures likewise contain an implicit `progn`. As a result, `progn` is not used as much as it was many years ago. It is needed now most often inside an `unwind-protect`, `and`, `or`, or in the `then`-part of an `if`.

*   Special Form: **progn** *forms…*

    This special form evaluates all of the `forms`, in textual order, returning the result of the final form.

        (progn (print "The first form")
               (print "The second form")
               (print "The third form"))
             -| "The first form"
             -| "The second form"
             -| "The third form"
        ⇒ "The third form"

Two other constructs likewise evaluate a series of forms but return different values:

*   Special Form: **prog1** *form1 forms…*

    This special form evaluates `form1` and all of the `forms`, in textual order, returning the result of `form1`.

        (prog1 (print "The first form")
               (print "The second form")
               (print "The third form"))
             -| "The first form"
             -| "The second form"
             -| "The third form"
        ⇒ "The first form"

    Here is a way to remove the first element from a list in the variable `x`, then return the value of that former element:

        (prog1 (car x) (setq x (cdr x)))

<!---->

*   Special Form: **prog2** *form1 form2 forms…*

    This special form evaluates `form1`, `form2`, and all of the following `forms`, in textual order, returning the result of `form2`.

        (prog2 (print "The first form")
               (print "The second form")
               (print "The third form"))
             -| "The first form"
             -| "The second form"
             -| "The third form"
        ⇒ "The second form"

Next: [Conditionals](Conditionals.html), Up: [Control Structures](Control-Structures.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
