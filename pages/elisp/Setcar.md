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

Next: [Setcdr](Setcdr.html), Up: [Modifying Lists](Modifying-Lists.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 5.6.1 Altering List Elements with `setcar`

Changing the CAR of a cons cell is done with `setcar`. When used on a list, `setcar` replaces one element of a list with a different element.

*   Function: **setcar** *cons object*

    This function stores `object` as the new CAR of `cons`, replacing its previous CAR. In other words, it changes the CAR slot of `cons` to refer to `object`. It returns the value `object`. For example:

        (setq x (list 1 2))
             ⇒ (1 2)

    <!---->

        (setcar x 4)
             ⇒ 4

    <!---->

        x
             ⇒ (4 2)

When a cons cell is part of the shared structure of several lists, storing a new CAR into the cons changes one element of each of these lists. Here is an example:

    ;; Create two lists that are partly shared.
    (setq x1 (list 'a 'b 'c))
         ⇒ (a b c)
    (setq x2 (cons 'z (cdr x1)))
         ⇒ (z b c)

```
```

    ;; Replace the CAR of a shared link.
    (setcar (cdr x1) 'foo)
         ⇒ foo
    x1                           ; Both lists are changed.
         ⇒ (a foo c)
    x2
         ⇒ (z foo c)

```
```

    ;; Replace the CAR of a link that is not shared.
    (setcar x1 'baz)
         ⇒ baz
    x1                           ; Only one list is changed.
         ⇒ (baz foo c)
    x2
         ⇒ (z foo c)

Here is a graphical depiction of the shared structure of the two lists in the variables `x1` and `x2`, showing why replacing `b` changes them both:

            --- ---        --- ---      --- ---
    x1---> |   |   |----> |   |   |--> |   |   |--> nil
            --- ---        --- ---      --- ---
             |        -->   |            |
             |       |      |            |
              --> a  |       --> b        --> c
                     |
           --- ---   |
    x2--> |   |   |--
           --- ---
            |
            |
             --> z

Here is an alternative form of box diagram, showing the same relationship:

    x1:
     --------------       --------------       --------------
    | car   | cdr  |     | car   | cdr  |     | car   | cdr  |
    |   a   |   o------->|   b   |   o------->|   c   |  nil |
    |       |      |  -->|       |      |     |       |      |
     --------------  |    --------------       --------------
                     |
    x2:              |
     --------------  |
    | car   | cdr  | |
    |   z   |   o----
    |       |      |
     --------------

Next: [Setcdr](Setcdr.html), Up: [Modifying Lists](Modifying-Lists.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
