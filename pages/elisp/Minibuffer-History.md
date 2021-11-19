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

Next: [Initial Input](Initial-Input.html), Previous: [Object from Minibuffer](Object-from-Minibuffer.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 20.4 Minibuffer History

A *minibuffer history list* records previous minibuffer inputs so the user can reuse them conveniently. It is a variable whose value is a list of strings (previous inputs), most recent first.

There are many separate minibuffer history lists, used for different kinds of inputs. It’s the Lisp programmer’s job to specify the right history list for each use of the minibuffer.

You specify a minibuffer history list with the optional `history` argument to `read-from-minibuffer` or `completing-read`. Here are the possible values for it:

*   `variable`

    Use `variable` (a symbol) as the history list.

*   (`variable` . `startpos`)

    Use `variable` (a symbol) as the history list, and assume that the initial history position is `startpos` (a nonnegative integer).

    Specifying 0 for `startpos` is equivalent to just specifying the symbol `variable`. `previous-history-element` will display the most recent element of the history list in the minibuffer. If you specify a positive `startpos`, the minibuffer history functions behave as if `(elt variable (1- startpos))` were the history element currently shown in the minibuffer.

    For consistency, you should also specify that element of the history as the initial minibuffer contents, using the `initial` argument to the minibuffer input function (see [Initial Input](Initial-Input.html)).

If you don’t specify `history`, then the default history list `minibuffer-history` is used. For other standard history lists, see below. You can also create your own history list variable; just initialize it to `nil` before the first use. If the variable is buffer local, then each buffer will have its own input history list.

Both `read-from-minibuffer` and `completing-read` add new elements to the history list automatically, and provide commands to allow the user to reuse items on the list. The only thing your program needs to do to use a history list is to initialize it and to pass its name to the input functions when you wish. But it is safe to modify the list by hand when the minibuffer input functions are not using it.

Emacs functions that add a new element to a history list can also delete old elements if the list gets too long. The variable `history-length` specifies the maximum length for most history lists. To specify a different maximum length for a particular history list, put the length in the `history-length` property of the history list symbol. The variable `history-delete-duplicates` specifies whether to delete duplicates in history.

*   Function: **add-to-history** *history-var newelt \&optional maxelt keep-all*

    This function adds a new element `newelt`, if it isn’t the empty string, to the history list stored in the variable `history-var`, and returns the updated history list. It limits the list length to the value of `maxelt` (if non-`nil`) or `history-length` (described below). The possible values of `maxelt` have the same meaning as the values of `history-length`. `history-var` cannot refer to a lexical variable.

    Normally, `add-to-history` removes duplicate members from the history list if `history-delete-duplicates` is non-`nil`. However, if `keep-all` is non-`nil`, that says not to remove duplicates, and to add `newelt` to the list even if it is empty.

<!---->

*   Variable: **history-add-new-input**

    If the value of this variable is `nil`, standard functions that read from the minibuffer don’t add new elements to the history list. This lets Lisp programs explicitly manage input history by using `add-to-history`. The default value is `t`.

<!---->

*   User Option: **history-length**

    The value of this variable specifies the maximum length for all history lists that don’t specify their own maximum lengths. If the value is `t`, that means there is no maximum (don’t delete old elements). If a history list variable’s symbol has a non-`nil` `history-length` property, it overrides this variable for that particular history list.

<!---->

*   User Option: **history-delete-duplicates**

    If the value of this variable is `t`, that means when adding a new history element, all previous identical elements are deleted.

Here are some of the standard minibuffer history list variables:

*   Variable: **minibuffer-history**

    The default history list for minibuffer history input.

<!---->

*   Variable: **query-replace-history**

    A history list for arguments to `query-replace` (and similar arguments to other commands).

<!---->

*   Variable: **file-name-history**

    A history list for file-name arguments.

<!---->

*   Variable: **buffer-name-history**

    A history list for buffer-name arguments.

<!---->

*   Variable: **regexp-history**

    A history list for regular expression arguments.

<!---->

*   Variable: **extended-command-history**

    A history list for arguments that are names of extended commands.

<!---->

*   Variable: **shell-command-history**

    A history list for arguments that are shell commands.

<!---->

*   Variable: **read-expression-history**

    A history list for arguments that are Lisp expressions to evaluate.

<!---->

*   Variable: **face-name-history**

    A history list for arguments that are faces.

<!---->

*   Variable: **custom-variable-history**

    A history list for variable-name arguments read by `read-variable`.

Next: [Initial Input](Initial-Input.html), Previous: [Object from Minibuffer](Object-from-Minibuffer.html), Up: [Minibuffers](Minibuffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
