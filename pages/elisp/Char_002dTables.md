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

Next: [Bool-Vectors](Bool_002dVectors.html), Previous: [Vector Functions](Vector-Functions.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 6.6 Char-Tables

A char-table is much like a vector, except that it is indexed by character codes. Any valid character code, without modifiers, can be used as an index in a char-table. You can access a char-table’s elements with `aref` and `aset`, as with any array. In addition, a char-table can have *extra slots* to hold additional data not associated with particular character codes. Like vectors, char-tables are constants when evaluated, and can hold elements of any type.

Each char-table has a *subtype*, a symbol, which serves two purposes:

*   The subtype provides an easy way to tell what the char-table is for. For instance, display tables are char-tables with `display-table` as the subtype, and syntax tables are char-tables with `syntax-table` as the subtype. The subtype can be queried using the function `char-table-subtype`, described below.
*   The subtype controls the number of *extra slots* in the char-table. This number is specified by the subtype’s `char-table-extra-slots` symbol property (see [Symbol Properties](Symbol-Properties.html)), whose value should be an integer between 0 and 10. If the subtype has no such symbol property, the char-table has no extra slots.

A char-table can have a *parent*, which is another char-table. If it does, then whenever the char-table specifies `nil` for a particular character `c`, it inherits the value specified in the parent. In other words, `(aref char-table c)` returns the value from the parent of `char-table` if `char-table` itself specifies `nil`.

A char-table can also have a *default value*. If so, then `(aref char-table c)` returns the default value whenever the char-table does not specify any other non-`nil` value.

*   Function: **make-char-table** *subtype \&optional init*

    Return a newly-created char-table, with subtype `subtype` (a symbol). Each element is initialized to `init`, which defaults to `nil`. You cannot alter the subtype of a char-table after the char-table is created.

    There is no argument to specify the length of the char-table, because all char-tables have room for any valid character code as an index.

    If `subtype` has the `char-table-extra-slots` symbol property, that specifies the number of extra slots in the char-table. This should be an integer between 0 and 10; otherwise, `make-char-table` raises an error. If `subtype` has no `char-table-extra-slots` symbol property (see [Property Lists](Property-Lists.html)), the char-table has no extra slots.

<!---->

*   Function: **char-table-p** *object*

    This function returns `t` if `object` is a char-table, and `nil` otherwise.

<!---->

*   Function: **char-table-subtype** *char-table*

    This function returns the subtype symbol of `char-table`.

There is no special function to access default values in a char-table. To do that, use `char-table-range` (see below).

*   Function: **char-table-parent** *char-table*

    This function returns the parent of `char-table`. The parent is always either `nil` or another char-table.

<!---->

*   Function: **set-char-table-parent** *char-table new-parent*

    This function sets the parent of `char-table` to `new-parent`.

<!---->

*   Function: **char-table-extra-slot** *char-table n*

    This function returns the contents of extra slot `n` (zero based) of `char-table`. The number of extra slots in a char-table is determined by its subtype.

<!---->

*   Function: **set-char-table-extra-slot** *char-table n value*

    This function stores `value` in extra slot `n` (zero based) of `char-table`.

A char-table can specify an element value for a single character code; it can also specify a value for an entire character set.

*   Function: **char-table-range** *char-table range*

    This returns the value specified in `char-table` for a range of characters `range`. Here are the possibilities for `range`:

    *   `nil`

        Refers to the default value.

    *   `char`

        Refers to the element for character `char` (supposing `char` is a valid character code).

    *   `(from . to)`

        A cons cell refers to all the characters in the inclusive range ‘`[from..to]`’.

<!---->

*   Function: **set-char-table-range** *char-table range value*

    This function sets the value in `char-table` for a range of characters `range`. Here are the possibilities for `range`:

    *   `nil`

        Refers to the default value.

    *   `t`

        Refers to the whole range of character codes.

    *   `char`

        Refers to the element for character `char` (supposing `char` is a valid character code).

    *   `(from . to)`

        A cons cell refers to all the characters in the inclusive range ‘`[from..to]`’.

<!---->

*   Function: **map-char-table** *function char-table*

    This function calls its argument `function` for each element of `char-table` that has a non-`nil` value. The call to `function` is with two arguments, a key and a value. The key is a possible `range` argument for `char-table-range`—either a valid character or a cons cell `(from . to)`, specifying a range of characters that share the same value. The value is what `(char-table-range char-table key)` returns.

    Overall, the key-value pairs passed to `function` describe all the values stored in `char-table`.

    The return value is always `nil`; to make calls to `map-char-table` useful, `function` should have side effects. For example, here is how to examine the elements of the syntax table:

        (let (accumulator)
           (map-char-table
            (lambda (key value)
              (setq accumulator
                    (cons (list
                           (if (consp key)
                               (list (car key) (cdr key))
                             key)
                           value)
                          accumulator)))
            (syntax-table))
           accumulator)
        ⇒
        (((2597602 4194303) (2)) ((2597523 2597601) (3))
         ... (65379 (5 . 65378)) (65378 (4 . 65379)) (65377 (1))
         ... (12 (0)) (11 (3)) (10 (12)) (9 (0)) ((0 8) (3)))

Next: [Bool-Vectors](Bool_002dVectors.html), Previous: [Vector Functions](Vector-Functions.html), Up: [Sequences Arrays Vectors](Sequences-Arrays-Vectors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
