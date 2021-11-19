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

Next: [Sound Output](Sound-Output.html), Previous: [Terminal Input](Terminal-Input.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.14 Terminal Output

The terminal output functions send output to a text terminal, or keep track of output sent to the terminal. The variable `baud-rate` tells you what Emacs thinks is the output speed of the terminal.

*   User Option: **baud-rate**

    This variable’s value is the output speed of the terminal, as far as Emacs knows. Setting this variable does not change the speed of actual data transmission, but the value is used for calculations such as padding.

    It also affects decisions about whether to scroll part of the screen or repaint on text terminals. See [Forcing Redisplay](Forcing-Redisplay.html), for the corresponding functionality on graphical terminals.

    The value is measured in baud.

If you are running across a network, and different parts of the network work at different baud rates, the value returned by Emacs may be different from the value used by your local terminal. Some network protocols communicate the local terminal speed to the remote machine, so that Emacs and other programs can get the proper value, but others do not. If Emacs has the wrong value, it makes decisions that are less than optimal. To fix the problem, set `baud-rate`.

*   Function: **send-string-to-terminal** *string \&optional terminal*

    This function sends `string` to `terminal` without alteration. Control characters in `string` have terminal-dependent effects. (If you need to display non-ASCII text on the terminal, encode it using one of the functions described in [Explicit Encoding](Explicit-Encoding.html).) This function operates only on text terminals. `terminal` may be a terminal object, a frame, or `nil` for the selected frame’s terminal. In batch mode, `string` is sent to `stdout` when `terminal` is `nil`.

    One use of this function is to define function keys on terminals that have downloadable function key definitions. For example, this is how (on certain terminals) to define function key 4 to move forward four characters (by transmitting the characters `C-u C-f` to the computer):

        (send-string-to-terminal "\eF4\^U\^F")
             ⇒ nil

<!---->

*   Command: **open-termscript** *filename*

    This function is used to open a *termscript file* that will record all the characters sent by Emacs to the terminal. It returns `nil`. Termscript files are useful for investigating problems where Emacs garbles the screen, problems that are due to incorrect Termcap entries or to undesirable settings of terminal options more often than to actual Emacs bugs. Once you are certain which characters were actually output, you can determine reliably whether they correspond to the Termcap specifications in use.

        (open-termscript "../junk/termscript")
             ⇒ nil

    You close the termscript file by calling this function with an argument of `nil`.

    See also `open-dribble-file` in [Recording Input](Recording-Input.html).

Next: [Sound Output](Sound-Output.html), Previous: [Terminal Input](Terminal-Input.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
