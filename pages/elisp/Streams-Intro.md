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

Next: [Input Streams](Input-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 19.1 Introduction to Reading and Printing

*Reading* a Lisp object means parsing a Lisp expression in textual form and producing a corresponding Lisp object. This is how Lisp programs get into Lisp from files of Lisp code. We call the text the *read syntax* of the object. For example, the text ‘`(a . 5)`’ is the read syntax for a cons cell whose CAR is `a` and whose CDR is the number 5.

*Printing* a Lisp object means producing text that represents that object—converting the object to its *printed representation* (see [Printed Representation](Printed-Representation.html)). Printing the cons cell described above produces the text ‘`(a . 5)`’.

Reading and printing are more or less inverse operations: printing the object that results from reading a given piece of text often produces the same text, and reading the text that results from printing an object usually produces a similar-looking object. For example, printing the symbol `foo` produces the text ‘`foo`’, and reading that text returns the symbol `foo`. Printing a list whose elements are `a` and `b` produces the text ‘`(a b)`’, and reading that text produces a list (but not the same list) with elements `a` and `b`.

However, these two operations are not precisely inverse to each other. There are three kinds of exceptions:

*   Printing can produce text that cannot be read. For example, buffers, windows, frames, subprocesses and markers print as text that starts with ‘`#`’; if you try to read this text, you get an error. There is no way to read those data types.
*   One object can have multiple textual representations. For example, ‘`1`’ and ‘`01`’ represent the same integer, and ‘`(a b)`’ and ‘`(a . (b))`’ represent the same list. Reading will accept any of the alternatives, but printing must choose one of them.
*   Comments can appear at certain points in the middle of an object’s read sequence without affecting the result of reading it.

Next: [Input Streams](Input-Streams.html), Up: [Read and Print](Read-and-Print.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
