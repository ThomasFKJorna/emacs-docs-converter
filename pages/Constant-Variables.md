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

Next: [Local Variables](Local-Variables.html), Previous: [Global Variables](Global-Variables.html), Up: [Variables](Variables.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 12.2 Variables that Never Change

In Emacs Lisp, certain symbols normally evaluate to themselves. These include `nil` and `t`, as well as any symbol whose name starts with ‘`:`’ (these are called *keywords*). These symbols cannot be rebound, nor can their values be changed. Any attempt to set or bind `nil` or `t` signals a `setting-constant` error. The same is true for a keyword (a symbol whose name starts with ‘`:`’), if it is interned in the standard obarray, except that setting such a symbol to itself is not an error.

    nil ≡ 'nil
         ⇒ nil

<!---->

    (setq nil 500)
    error→ Attempt to set constant symbol: nil

*   Function: **keywordp** *object*

    function returns `t` if `object` is a symbol whose name starts with ‘`:`’, interned in the standard obarray, and returns `nil` otherwise.

These constants are fundamentally different from the constants defined using the `defconst` special form (see [Defining Variables](Defining-Variables.html)). A `defconst` form serves to inform human readers that you do not intend to change the value of a variable, but Emacs does not raise an error if you actually change it.

A small number of additional symbols are made read-only for various practical reasons. These include `enable-multibyte-characters`, `most-positive-fixnum`, `most-negative-fixnum`, and a few others. Any attempt to set or bind these also signals a `setting-constant` error.
