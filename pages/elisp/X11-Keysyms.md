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

Next: [Batch Mode](Batch-Mode.html), Previous: [Sound Output](Sound-Output.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.16 Operating on X11 Keysyms

To define system-specific X11 keysyms, set the variable `system-key-alist`.

*   Variable: **system-key-alist**

    This variable’s value should be an alist with one element for each system-specific keysym. Each element has the form `(code . symbol)`, where `code` is the numeric keysym code (not including the vendor-specific bit, -2\*\*28), and `symbol` is the name for the function key.

    For example `(168 . mute-acute)` defines a system-specific key (used by HP X servers) whose numeric code is -2\*\*28 + 168.

    It is not crucial to exclude from the alist the keysyms of other X servers; those do no harm, as long as they don’t conflict with the ones used by the X server actually in use.

    The variable is always local to the current terminal, and cannot be buffer-local. See [Multiple Terminals](Multiple-Terminals.html).

You can specify which keysyms Emacs should use for the Control, Meta, Alt, Hyper, and Super modifiers by setting these variables:

*   *   Variable: **x-ctrl-keysym**
    *   Variable: **x-alt-keysym**
    *   Variable: **x-meta-keysym**
    *   Variable: **x-hyper-keysym**
    *   Variable: **x-super-keysym**

    The name of the keysym that should stand for the Control modifier (respectively, for Alt, Meta, Hyper, and Super). For example, here is how to swap the Meta and Alt modifiers within Emacs:

        (setq x-alt-keysym 'meta)
        (setq x-meta-keysym 'alt)
