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

Next: [Help Functions](Help-Functions.html), Previous: [Text Quoting Style](Text-Quoting-Style.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 24.5 Describing Characters for Help Messages

These functions convert events, key sequences, or characters to textual descriptions. These descriptions are useful for including arbitrary text characters or key sequences in messages, because they convert non-printing and whitespace characters to sequences of printing characters. The description of a non-whitespace printing character is the character itself.

*   Function: **key-description** *sequence \&optional prefix*

    This function returns a string containing the Emacs standard notation for the input events in `sequence`. If `prefix` is non-`nil`, it is a sequence of input events leading up to `sequence` and is included in the return value. Both arguments may be strings, vectors or lists. See [Input Events](Input-Events.html), for more information about valid events.

        (key-description [?\M-3 delete])
             ⇒ "M-3 <delete>"

    <!---->

        (key-description [delete] "\M-3")
             ⇒ "M-3 <delete>"

    See also the examples for `single-key-description`, below.

<!---->

*   Function: **single-key-description** *event \&optional no-angles*

    This function returns a string describing `event` in the standard Emacs notation for keyboard input. A normal printing character appears as itself, but a control character turns into a string starting with ‘`C-`’, a meta character turns into a string starting with ‘`M-`’, and space, tab, etc., appear as ‘`SPC`’, ‘`TAB`’, etc. A function key symbol appears inside angle brackets ‘`<…>`’. An event that is a list appears as the name of the symbol in the CAR of the list, inside angle brackets.

    If the optional argument `no-angles` is non-`nil`, the angle brackets around function keys and event symbols are omitted; this is for compatibility with old versions of Emacs which didn’t use the brackets.

        (single-key-description ?\C-x)
             ⇒ "C-x"

    <!---->

        (key-description "\C-x \M-y \n \t \r \f123")
             ⇒ "C-x SPC M-y SPC C-j SPC TAB SPC RET SPC C-l 1 2 3"

    <!---->

        (single-key-description 'delete)
             ⇒ "<delete>"

    <!---->

        (single-key-description 'C-mouse-1)
             ⇒ "<C-mouse-1>"

    <!---->

        (single-key-description 'C-mouse-1 t)
             ⇒ "C-mouse-1"

<!---->

*   Function: **text-char-description** *character*

    This function returns a string describing `character` in the standard Emacs notation for characters that can appear in text—similar to `single-key-description`, except that the argument must be a valid character code that passes a `characterp` test (see [Character Codes](Character-Codes.html)). The function produces descriptions of control characters with a leading caret (which is how Emacs usually displays control characters in buffers). Characters with modifier bits will cause this function to signal an error (ASCII characters with the Control modifier are an exception, they are represented as control characters).

        (text-char-description ?\C-c)
             ⇒ "^C"

    <!---->

        (text-char-description ?\M-m)
             error→ Wrong type argument: characterp, 134217837

<!---->

*   Command: **read-kbd-macro** *string \&optional need-vector*

    This function is used mainly for operating on keyboard macros, but it can also be used as a rough inverse for `key-description`. You call it with a string containing key descriptions, separated by spaces; it returns a string or vector containing the corresponding events. (This may or may not be a single valid key sequence, depending on what events you use; see [Key Sequences](Key-Sequences.html).) If `need-vector` is non-`nil`, the return value is always a vector.

Next: [Help Functions](Help-Functions.html), Previous: [Text Quoting Style](Text-Quoting-Style.html), Up: [Documentation](Documentation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
