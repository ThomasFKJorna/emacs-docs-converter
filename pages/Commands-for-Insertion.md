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

Next: [Deletion](Deletion.html), Previous: [Insertion](Insertion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 32.5 User-Level Insertion Commands

This section describes higher-level commands for inserting text, commands intended primarily for the user but useful also in Lisp programs.

*   Command: **insert-buffer** *from-buffer-or-name*

    This command inserts the entire accessible contents of `from-buffer-or-name` (which must exist) into the current buffer after point. It leaves the mark after the inserted text. The value is `nil`.

<!---->

*   Command: **self-insert-command** *count \&optional char*

    This command inserts the character `char` (the last character typed); it does so `count` times, before point, and returns `nil`. Most printing characters are bound to this command. In routine use, `self-insert-command` is the most frequently called function in Emacs, but programs rarely use it except to install it on a keymap.

    In an interactive call, `count` is the numeric prefix argument.

    Self-insertion translates the input character through `translation-table-for-input`. See [Translation of Characters](Translation-of-Characters.html).

    This command calls `auto-fill-function` whenever that is non-`nil` and the character inserted is in the table `auto-fill-chars` (see [Auto Filling](Auto-Filling.html)).

    This command performs abbrev expansion if Abbrev mode is enabled and the inserted character does not have word-constituent syntax. (See [Abbrevs](Abbrevs.html), and [Syntax Class Table](Syntax-Class-Table.html).) It is also responsible for calling `blink-paren-function` when the inserted character has close parenthesis syntax (see [Blinking](Blinking.html)).

    The final thing this command does is to run the hook `post-self-insert-hook`. You could use this to automatically reindent text as it is typed, for example. If any function on this hook needs to act on the region (see [The Region](The-Region.html)), it should make sure Delete Selection mode (see [Delete Selection](https://www.gnu.org/software/emacs/manual/html_node/emacs/Using-Region.html#Using-Region) in The GNU Emacs Manual) doesn’t delete the region before `post-self-insert-hook` functions are invoked. The way to do so is to add a function that returns `nil` to `self-insert-uses-region-functions`, a special hook that tells Delete Selection mode it should not delete the region.

    Do not try substituting your own definition of `self-insert-command` for the standard one. The editor command loop handles this function specially.

<!---->

*   Command: **newline** *\&optional number-of-newlines interactive*

    This command inserts newlines into the current buffer before point. If `number-of-newlines` is supplied, that many newline characters are inserted. In an interactive call, `number-of-newlines` is the numeric prefix argument.

    This command calls `self-insert-command` to insert newlines, which may subsequently break the preceding line by calling `auto-fill-function` (see [Auto Filling](Auto-Filling.html)). Typically what `auto-fill-function` does is insert a newline; thus, the overall result in this case is to insert two newlines at different places: one at point, and another earlier in the line. `newline` does not auto-fill if `number-of-newlines` is non-`nil`.

    This command does not run the hook `post-self-insert-hook` unless called interactively or `interactive` is non-`nil`.

    This command indents to the left margin if that is not zero. See [Margins](Margins.html).

    The value returned is `nil`.

<!---->

*   Variable: **overwrite-mode**

    This variable controls whether overwrite mode is in effect. The value should be `overwrite-mode-textual`, `overwrite-mode-binary`, or `nil`. `overwrite-mode-textual` specifies textual overwrite mode (treats newlines and tabs specially), and `overwrite-mode-binary` specifies binary overwrite mode (treats newlines and tabs like any other characters).

Next: [Deletion](Deletion.html), Previous: [Insertion](Insertion.html), Up: [Text](Text.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
