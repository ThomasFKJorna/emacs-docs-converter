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

Next: [Recursive Editing](Recursive-Editing.html), Previous: [Quitting](Quitting.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 21.12 Prefix Command Arguments

Most Emacs commands can use a *prefix argument*, a number specified before the command itself. (Don’t confuse prefix arguments with prefix keys.) The prefix argument is at all times represented by a value, which may be `nil`, meaning there is currently no prefix argument. Each command may use the prefix argument or ignore it.

There are two representations of the prefix argument: *raw* and *numeric*. The editor command loop uses the raw representation internally, and so do the Lisp variables that store the information, but commands can request either representation.

Here are the possible values of a raw prefix argument:

*   `nil`, meaning there is no prefix argument. Its numeric value is 1, but numerous commands make a distinction between `nil` and the integer 1.
*   An integer, which stands for itself.
*   A list of one element, which is an integer. This form of prefix argument results from one or a succession of `C-u`s with no digits. The numeric value is the integer in the list, but some commands make a distinction between such a list and an integer alone.
*   The symbol `-`. This indicates that `M--` or `C-u -` was typed, without following digits. The equivalent numeric value is -1, but some commands make a distinction between the integer -1 and the symbol `-`.

We illustrate these possibilities by calling the following function with various prefixes:

    (defun display-prefix (arg)
      "Display the value of the raw prefix arg."
      (interactive "P")
      (message "%s" arg))

Here are the results of calling `display-prefix` with various raw prefix arguments:

            M-x display-prefix  -| nil

    C-u     M-x display-prefix  -| (4)

    C-u C-u M-x display-prefix  -| (16)

    C-u 3   M-x display-prefix  -| 3

    M-3     M-x display-prefix  -| 3      ; (Same as C-u 3.)

    C-u -   M-x display-prefix  -| -

    M--     M-x display-prefix  -| -      ; (Same as C-u -.)

    C-u - 7 M-x display-prefix  -| -7

    M-- 7   M-x display-prefix  -| -7     ; (Same as C-u -7.)

Emacs uses two variables to store the prefix argument: `prefix-arg` and `current-prefix-arg`. Commands such as `universal-argument` that set up prefix arguments for other commands store them in `prefix-arg`. In contrast, `current-prefix-arg` conveys the prefix argument to the current command, so setting it has no effect on the prefix arguments for future commands.

Normally, commands specify which representation to use for the prefix argument, either numeric or raw, in the `interactive` specification. (See [Using Interactive](Using-Interactive.html).) Alternatively, functions may look at the value of the prefix argument directly in the variable `current-prefix-arg`, but this is less clean.

*   Function: **prefix-numeric-value** *arg*

    This function returns the numeric meaning of a valid raw prefix argument value, `arg`. The argument may be a symbol, a number, or a list. If it is `nil`, the value 1 is returned; if it is `-`, the value -1 is returned; if it is a number, that number is returned; if it is a list, the CAR of that list (which should be a number) is returned.

<!---->

*   Variable: **current-prefix-arg**

    This variable holds the raw prefix argument for the *current* command. Commands may examine it directly, but the usual method for accessing it is with `(interactive "P")`.

<!---->

*   Variable: **prefix-arg**

    The value of this variable is the raw prefix argument for the *next* editing command. Commands such as `universal-argument` that specify prefix arguments for the following command work by setting this variable.

<!---->

*   Variable: **last-prefix-arg**

    The raw prefix argument value used by the previous command.

The following commands exist to set up prefix arguments for the following command. Do not call them for any other reason.

*   Command: **universal-argument**

    This command reads input and specifies a prefix argument for the following command. Don’t call this command yourself unless you know what you are doing.

<!---->

*   Command: **digit-argument** *arg*

    This command adds to the prefix argument for the following command. The argument `arg` is the raw prefix argument as it was before this command; it is used to compute the updated prefix argument. Don’t call this command yourself unless you know what you are doing.

<!---->

*   Command: **negative-argument** *arg*

    This command adds to the numeric argument for the next command. The argument `arg` is the raw prefix argument as it was before this command; its value is negated to form the new prefix argument. Don’t call this command yourself unless you know what you are doing.

Next: [Recursive Editing](Recursive-Editing.html), Previous: [Quitting](Quitting.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
