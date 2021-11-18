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

Next: [Modification Time](Modification-Time.html), Previous: [Buffer File Name](Buffer-File-Name.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.5 Buffer Modification

Emacs keeps a flag called the *modified flag* for each buffer, to record whether you have changed the text of the buffer. This flag is set to `t` whenever you alter the contents of the buffer, and cleared to `nil` when you save it. Thus, the flag shows whether there are unsaved changes. The flag value is normally shown in the mode line (see [Mode Line Variables](Mode-Line-Variables.html)), and controls saving (see [Saving Buffers](Saving-Buffers.html)) and auto-saving (see [Auto-Saving](Auto_002dSaving.html)).

Some Lisp programs set the flag explicitly. For example, the function `set-visited-file-name` sets the flag to `t`, because the text does not match the newly-visited file, even if it is unchanged from the file formerly visited.

The functions that modify the contents of buffers are described in [Text](Text.html).

*   Function: **buffer-modified-p** *\&optional buffer*

    This function returns `t` if the buffer `buffer` has been modified since it was last read in from a file or saved, or `nil` otherwise. If `buffer` is not supplied, the current buffer is tested.

<!---->

*   Function: **set-buffer-modified-p** *flag*

    This function marks the current buffer as modified if `flag` is non-`nil`, or as unmodified if the flag is `nil`.

    Another effect of calling this function is to cause unconditional redisplay of the mode line for the current buffer. In fact, the function `force-mode-line-update` works by doing this:

        (set-buffer-modified-p (buffer-modified-p))

<!---->

*   Function: **restore-buffer-modified-p** *flag*

    Like `set-buffer-modified-p`, but does not force redisplay of mode lines.

<!---->

*   Command: **not-modified** *\&optional arg*

    This command marks the current buffer as unmodified, and not needing to be saved. If `arg` is non-`nil`, it marks the buffer as modified, so that it will be saved at the next suitable occasion. Interactively, `arg` is the prefix argument.

    Don’t use this function in programs, since it prints a message in the echo area; use `set-buffer-modified-p` (above) instead.

<!---->

*   Function: **buffer-modified-tick** *\&optional buffer*

    This function returns `buffer`’s modification-count. This is a counter that increments every time the buffer is modified. If `buffer` is `nil` (or omitted), the current buffer is used.

<!---->

*   Function: **buffer-chars-modified-tick** *\&optional buffer*

    This function returns `buffer`’s character-change modification-count. Changes to text properties leave this counter unchanged; however, each time text is inserted or removed from the buffer, the counter is reset to the value that would be returned by `buffer-modified-tick`. By comparing the values returned by two `buffer-chars-modified-tick` calls, you can tell whether a character change occurred in that buffer in between the calls. If `buffer` is `nil` (or omitted), the current buffer is used.

Sometimes there’s a need for modifying buffer in a way that doesn’t really change its text, like if only its text properties are changed. If your program needs to modify a buffer without triggering any hooks and features that react to buffer modifications, use the `with-silent-modifications` macro.

*   Macro: **with-silent-modifications** *body…*

    Execute `body` pretending it does not modify the buffer. This includes checking whether the buffer’s file is locked (see [File Locks](File-Locks.html)), running buffer modification hooks (see [Change Hooks](Change-Hooks.html)), etc. Note that if `body` actually modifies the buffer text (as opposed to its text properties), its undo data may become corrupted.

Next: [Modification Time](Modification-Time.html), Previous: [Buffer File Name](Buffer-File-Name.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
