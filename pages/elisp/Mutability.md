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

Previous: [Equality Predicates](Equality-Predicates.html), Up: [Lisp Data Types](Lisp-Data-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 2.9 Mutability

Some Lisp objects should never change. For example, the Lisp expression `"aaa"` yields a string, but you should not change its contents. And some objects cannot be changed; for example, although you can create a new number by calculating one, Lisp provides no operation to change the value of an existing number.

Other Lisp objects are *mutable*: it is safe to change their values via destructive operations involving side effects. For example, an existing marker can be changed by moving the marker to point to somewhere else.

Although numbers never change and all markers are mutable, some types have members some of which are mutable and others not. These types include conses, vectors, and strings. For example, although `"cons"` and `(symbol-name 'cons)` both yield strings that should not be changed, `(copy-sequence "cons")` and `(make-string 3 ?a)` both yield mutable strings that can be changed via later calls to `aset`.

A mutable object stops being mutable if it is part of an expression that is evaluated. For example:

    (let* ((x (list 0.5))
           (y (eval (list 'quote x))))
      (setcar x 1.5) ;; The program should not do this.
      y)

Although the list `(0.5)` was mutable when it was created, it should not have been changed via `setcar` because it given to `eval`. The reverse does not occur: an object that should not be changed never becomes mutable afterwards.

If a program attempts to change objects that should not be changed, the resulting behavior is undefined: the Lisp interpreter might signal an error, or it might crash or behave unpredictably in other ways.[2](#FOOT2)

When similar constants occur as parts of a program, the Lisp interpreter might save time or space by reusing existing constants or their components. For example, `(eq "abc" "abc")` returns `t` if the interpreter creates only one instance of the string literal `"abc"`, and returns `nil` if it creates two instances. Lisp programs should be written so that they work regardless of whether this optimization is in use.

***

#### Footnotes

##### [(2)](#DOCF2)

This is the behavior specified for languages like Common Lisp and C for constants, and this differs from languages like JavaScript and Python where an interpreter is required to signal an error if a program attempts to change an immutable object. Ideally the Emacs Lisp interpreter will evolve in latter direction.

Previous: [Equality Predicates](Equality-Predicates.html), Up: [Lisp Data Types](Lisp-Data-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
