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

Next: [Vectors](Vectors.html), Previous: [Arrays](Arrays.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.3 Functions that Operate on Arrays

In this section, we describe the functions that accept all types of arrays.

*   Function: **arrayp** *object*

    This function returns `t` if `object` is an array (i.e., a vector, a string, a bool-vector or a char-table).

        (arrayp [a])
             ⇒ t
        (arrayp "asdf")
             ⇒ t
        (arrayp (syntax-table))    ;; A char-table.
             ⇒ t

<!---->

*   Function: **aref** *arr index*

    This function returns the `index`th element of the array or record `arr`. The first element is at index zero.

        (setq primes [2 3 5 7 11 13])
             ⇒ [2 3 5 7 11 13]
        (aref primes 4)
             ⇒ 11

    <!---->

        (aref "abcdefg" 1)
             ⇒ 98           ; ‘b’ is ASCII code 98.

    See also the function `elt`, in [Sequence Functions](Sequence-Functions.html).

<!---->

*   Function: **aset** *array index object*

    This function sets the `index`th element of `array` to be `object`. It returns `object`.

        (setq w (vector 'foo 'bar 'baz))
             ⇒ [foo bar baz]
        (aset w 0 'fu)
             ⇒ fu
        w
             ⇒ [fu bar baz]

    ```
    ```

        ;; copy-sequence copies the string to be modified later.
        (setq x (copy-sequence "asdfasfd"))
             ⇒ "asdfasfd"
        (aset x 3 ?Z)
             ⇒ 90
        x
             ⇒ "asdZasfd"

    The `array` should be mutable. See [Mutability](Mutability.html).

    If `array` is a string and `object` is not a character, a `wrong-type-argument` error results. The function converts a unibyte string to multibyte if necessary to insert a character.

<!---->

*   Function: **fillarray** *array object*

    This function fills the array `array` with `object`, so that each element of `array` is `object`. It returns `array`.

        (setq a (copy-sequence [a b c d e f g]))
             ⇒ [a b c d e f g]
        (fillarray a 0)
             ⇒ [0 0 0 0 0 0 0]
        a
             ⇒ [0 0 0 0 0 0 0]

    <!---->

        (setq s (copy-sequence "When in the course"))
             ⇒ "When in the course"
        (fillarray s ?-)
             ⇒ "------------------"

    If `array` is a string and `object` is not a character, a `wrong-type-argument` error results.

The general sequence functions `copy-sequence` and `length` are often useful for objects known to be arrays. See [Sequence Functions](Sequence-Functions.html).

Next: [Vectors](Vectors.html), Previous: [Arrays](Arrays.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
