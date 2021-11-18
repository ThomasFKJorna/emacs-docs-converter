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

Next: [Recording Input](Recording-Input.html), Up: [Terminal Input](Terminal-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 40.13.1 Input Modes

*   Function: **set-input-mode** *interrupt flow meta \&optional quit-char*

    This function sets the mode for reading keyboard input. If `interrupt` is non-`nil`, then Emacs uses input interrupts. If it is `nil`, then it uses CBREAK mode. The default setting is system-dependent. Some systems always use CBREAK mode regardless of what is specified.

    When Emacs communicates directly with X, it ignores this argument and uses interrupts if that is the way it knows how to communicate.

    If `flow` is non-`nil`, then Emacs uses XON/XOFF (`C-q`, `C-s`) flow control for output to the terminal. This has no effect except in CBREAK mode.

    The argument `meta` controls support for input character codes above 127. If `meta` is `t`, Emacs converts characters with the 8th bit set into Meta characters. If `meta` is `nil`, Emacs disregards the 8th bit; this is necessary when the terminal uses it as a parity bit. If `meta` is neither `t` nor `nil`, Emacs uses all 8 bits of input unchanged. This is good for terminals that use 8-bit character sets.

    If `quit-char` is non-`nil`, it specifies the character to use for quitting. Normally this character is `C-g`. See [Quitting](Quitting.html).

The `current-input-mode` function returns the input mode settings Emacs is currently using.

*   Function: **current-input-mode**

    This function returns the current mode for reading keyboard input. It returns a list, corresponding to the arguments of `set-input-mode`, of the form `(interrupt flow meta quit)` in which:

    *   `interrupt`

        is non-`nil` when Emacs is using interrupt-driven input. If `nil`, Emacs is using CBREAK mode.

    *   `flow`

        is non-`nil` if Emacs uses XON/XOFF (`C-q`, `C-s`) flow control for output to the terminal. This value is meaningful only when `interrupt` is `nil`.

    *   `meta`

        is `t` if Emacs treats the eighth bit of input characters as the meta bit; `nil` means Emacs clears the eighth bit of every input character; any other value means Emacs uses all eight bits as the basic character code.

    *   `quit`

        is the character Emacs currently uses for quitting, usually `C-g`.

Next: [Recording Input](Recording-Input.html), Up: [Terminal Input](Terminal-Input.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
