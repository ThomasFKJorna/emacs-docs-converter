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

Next: [Syntax Properties](Syntax-Properties.html), Previous: [Syntax Descriptors](Syntax-Descriptors.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 35.3 Syntax Table Functions

In this section we describe functions for creating, accessing and altering syntax tables.

*   Function: **make-syntax-table** *\&optional table*

    This function creates a new syntax table. If `table` is non-`nil`, the parent of the new syntax table is `table`; otherwise, the parent is the standard syntax table.

    In the new syntax table, all characters are initially given the “inherit” (‘`@`’) syntax class, i.e., their syntax is inherited from the parent table (see [Syntax Class Table](Syntax-Class-Table.html)).

<!---->

*   Function: **copy-syntax-table** *\&optional table*

    This function constructs a copy of `table` and returns it. If `table` is omitted or `nil`, it returns a copy of the standard syntax table. Otherwise, an error is signaled if `table` is not a syntax table.

<!---->

*   Command: **modify-syntax-entry** *char syntax-descriptor \&optional table*

    This function sets the syntax entry for `char` according to `syntax-descriptor`. `char` must be a character, or a cons cell of the form `(min . max)`; in the latter case, the function sets the syntax entries for all characters in the range between `min` and `max`, inclusive.

    The syntax is changed only for `table`, which defaults to the current buffer’s syntax table, and not in any other syntax table.

    The argument `syntax-descriptor` is a syntax descriptor, i.e., a string whose first character is a syntax class designator and whose second and subsequent characters optionally specify a matching character and syntax flags. See [Syntax Descriptors](Syntax-Descriptors.html). An error is signaled if `syntax-descriptor` is not a valid syntax descriptor.

    This function always returns `nil`. The old syntax information in the table for this character is discarded.

        Examples:

    ```

    ;; Put the space character in class whitespace.
    (modify-syntax-entry ?\s " ")
         ⇒ nil
    ```

    ```
    ```

        ;; Make ‘$’ an open parenthesis character,
        ;;   with ‘^’ as its matching close.
        (modify-syntax-entry ?$ "(^")
             ⇒ nil

    ```
    ```

        ;; Make ‘^’ a close parenthesis character,
        ;;   with ‘$’ as its matching open.
        (modify-syntax-entry ?^ ")$")
             ⇒ nil

    ```
    ```

        ;; Make ‘/’ a punctuation character,
        ;;   the first character of a start-comment sequence,
        ;;   and the second character of an end-comment sequence.
        ;;   This is used in C mode.
        (modify-syntax-entry ?/ ". 14")
             ⇒ nil

<!---->

*   Function: **char-syntax** *character*

    This function returns the syntax class of `character`, represented by its designator character (see [Syntax Class Table](Syntax-Class-Table.html)). This returns *only* the class, not its matching character or syntax flags.

    The following examples apply to C mode. (We use `string` to make it easier to see the character returned by `char-syntax`.)

        ;; Space characters have whitespace syntax class.
        (string (char-syntax ?\s))
             ⇒ " "

    ```
    ```

        ;; Forward slash characters have punctuation syntax.
        ;; Note that this char-syntax call does not reveal
        ;; that it is also part of comment-start and -end sequences.
        (string (char-syntax ?/))
             ⇒ "."

    ```
    ```

        ;; Open parenthesis characters have open parenthesis syntax.
        ;; Note that this char-syntax call does not reveal that
        ;; it has a matching character, ‘)’.
        (string (char-syntax ?\())
             ⇒ "("

<!---->

*   Function: **set-syntax-table** *table*

    This function makes `table` the syntax table for the current buffer. It returns `table`.

<!---->

*   Function: **syntax-table**

    This function returns the current syntax table, which is the table for the current buffer.

<!---->

*   Command: **describe-syntax** *\&optional buffer*

    This command displays the contents of the syntax table of `buffer` (by default, the current buffer) in a help buffer.

<!---->

*   Macro: **with-syntax-table** *table body…*

    This macro executes `body` using `table` as the current syntax table. It returns the value of the last form in `body`, after restoring the old current syntax table.

    Since each buffer has its own current syntax table, we should make that more precise: `with-syntax-table` temporarily alters the current syntax table of whichever buffer is current at the time the macro execution starts. Other buffers are not affected.

Next: [Syntax Properties](Syntax-Properties.html), Previous: [Syntax Descriptors](Syntax-Descriptors.html), Up: [Syntax Tables](Syntax-Tables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
