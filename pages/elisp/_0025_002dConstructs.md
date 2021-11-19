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

Next: [Properties in Mode](Properties-in-Mode.html), Previous: [Mode Line Variables](Mode-Line-Variables.html), Up: [Mode Line Format](Mode-Line-Format.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.4.5 `%`-Constructs in the Mode Line

Strings used as mode line constructs can use certain `%`-constructs to substitute various kinds of data. The following is a list of the defined `%`-constructs, and what they mean.

In any construct except ‘`%%`’, you can add a decimal integer after the ‘`%`’ to specify a minimum field width. If the width is less, the field is padded to that width. Purely numeric constructs (‘`c`’, ‘`i`’, ‘`I`’, and ‘`l`’) are padded by inserting spaces to the left, and others are padded by inserting spaces to the right.

*   `%b`

    The current buffer name, obtained with the `buffer-name` function. See [Buffer Names](Buffer-Names.html).

*   `%c`

    The current column number of point, counting from zero starting at the left margin of the window.

*   `%C`

    The current column number of point, counting from one starting at the left margin of the window.

*   `%e`

    When Emacs is nearly out of memory for Lisp objects, a brief message saying so. Otherwise, this is empty.

*   `%f`

    The visited file name, obtained with the `buffer-file-name` function. See [Buffer File Name](Buffer-File-Name.html).

*   `%F`

    The title (only on a window system) or the name of the selected frame. See [Basic Parameters](Basic-Parameters.html).

*   `%i`

    The size of the accessible part of the current buffer; basically `(- (point-max) (point-min))`.

*   `%I`

    Like ‘`%i`’, but the size is printed in a more readable way by using ‘`k`’ for 10^3, ‘`M`’ for 10^6, ‘`G`’ for 10^9, etc., to abbreviate.

*   `%l`

    The current line number of point, counting within the accessible portion of the buffer.

*   `%n`

    ‘`Narrow`’ when narrowing is in effect; nothing otherwise (see `narrow-to-region` in [Narrowing](Narrowing.html)).

*   `%o`

    The degree of *travel* of the window through (the visible portion of) the buffer, i.e. the size of the text above the top of the window expressed as a percentage of all the text outside the window, or ‘`Top`’, ‘`Bottom`’ or ‘`All`’.

*   `%p`

    The percentage of the buffer text above the **top** of window, or ‘`Top`’, ‘`Bottom`’ or ‘`All`’. Note that the default mode line construct truncates this to three characters.

*   `%P`

    The percentage of the buffer text that is above the **bottom** of the window (which includes the text visible in the window, as well as the text above the top), plus ‘`Top`’ if the top of the buffer is visible on screen; or ‘`Bottom`’ or ‘`All`’.

*   `%q`

    The percentages of text above both the **top** and the **bottom** of the window, separated by ‘`-`’, or ‘`All`’.

*   `%s`

    The status of the subprocess belonging to the current buffer, obtained with `process-status`. See [Process Information](Process-Information.html).

*   `%z`

    The mnemonics of keyboard, terminal, and buffer coding systems.

*   `%Z`

    Like ‘`%z`’, but including the end-of-line format.

*   `%*`

    ‘`%`’ if the buffer is read only (see `buffer-read-only`);\
    ‘`*`’ if the buffer is modified (see `buffer-modified-p`);\
    ‘`-`’ otherwise. See [Buffer Modification](Buffer-Modification.html).

*   `%+`

    ‘`*`’ if the buffer is modified (see `buffer-modified-p`);\
    ‘`%`’ if the buffer is read only (see `buffer-read-only`);\
    ‘`-`’ otherwise. This differs from ‘`%*`’ only for a modified read-only buffer. See [Buffer Modification](Buffer-Modification.html).

*   `%&`

    ‘`*`’ if the buffer is modified, and ‘`-`’ otherwise.

*   `%@`

    ‘`@`’ if the buffer’s `default-directory` (see [File Name Expansion](File-Name-Expansion.html)) is on a remote machine, and ‘`-`’ otherwise.

*   `%[`

    An indication of the depth of recursive editing levels (not counting minibuffer levels): one ‘`[`’ for each editing level. See [Recursive Editing](Recursive-Editing.html).

*   `%]`

    One ‘`]`’ for each recursive editing level (not counting minibuffer levels).

*   `%-`

    Dashes sufficient to fill the remainder of the mode line.

*   `%%`

    The character ‘`%`’—this is how to include a literal ‘`%`’ in a string in which `%`-constructs are allowed.

The following `%`-construct is still supported, but it is obsolete, since you can get the same result using the variable `mode-name`.

*   `%m`

    The value of `mode-name`.

Next: [Properties in Mode](Properties-in-Mode.html), Previous: [Mode Line Variables](Mode-Line-Variables.html), Up: [Mode Line Format](Mode-Line-Format.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
