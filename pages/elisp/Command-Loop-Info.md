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

Next: [Adjusting Point](Adjusting-Point.html), Previous: [Distinguish Interactive](Distinguish-Interactive.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 21.5 Information from the Command Loop

The editor command loop sets several Lisp variables to keep status records for itself and for commands that are run. With the exception of `this-command` and `last-command` it’s generally a bad idea to change any of these variables in a Lisp program.

*   Variable: **last-command**

    This variable records the name of the previous command executed by the command loop (the one before the current command). Normally the value is a symbol with a function definition, but this is not guaranteed.

    The value is copied from `this-command` when a command returns to the command loop, except when the command has specified a prefix argument for the following command.

    This variable is always local to the current terminal and cannot be buffer-local. See [Multiple Terminals](Multiple-Terminals.html).

<!---->

*   Variable: **real-last-command**

    This variable is set up by Emacs just like `last-command`, but never altered by Lisp programs.

<!---->

*   Variable: **last-repeatable-command**

    This variable stores the most recently executed command that was not part of an input event. This is the command `repeat` will try to repeat, See [Repeating](https://www.gnu.org/software/emacs/manual/html_node/emacs/Repeating.html#Repeating) in The GNU Emacs Manual.

<!---->

*   Variable: **this-command**

    This variable records the name of the command now being executed by the editor command loop. Like `last-command`, it is normally a symbol with a function definition.

    The command loop sets this variable just before running a command, and copies its value into `last-command` when the command finishes (unless the command specified a prefix argument for the following command).

    Some commands set this variable during their execution, as a flag for whatever command runs next. In particular, the functions for killing text set `this-command` to `kill-region` so that any kill commands immediately following will know to append the killed text to the previous kill.

If you do not want a particular command to be recognized as the previous command in the case where it got an error, you must code that command to prevent this. One way is to set `this-command` to `t` at the beginning of the command, and set `this-command` back to its proper value at the end, like this:

    (defun foo (args…)
      (interactive …)
      (let ((old-this-command this-command))
        (setq this-command t)
        …do the work…
        (setq this-command old-this-command)))

We do not bind `this-command` with `let` because that would restore the old value in case of error—a feature of `let` which in this case does precisely what we want to avoid.

*   Variable: **this-original-command**

    This has the same value as `this-command` except when command remapping occurs (see [Remapping Commands](Remapping-Commands.html)). In that case, `this-command` gives the command actually run (the result of remapping), and `this-original-command` gives the command that was specified to run but remapped into another command.

<!---->

*   Function: **this-command-keys**

    This function returns a string or vector containing the key sequence that invoked the present command, plus any previous commands that generated the prefix argument for this command. Any events read by the command using `read-event` without a timeout get tacked on to the end.

    However, if the command has called `read-key-sequence`, it returns the last read key sequence. See [Key Sequence Input](Key-Sequence-Input.html). The value is a string if all events in the sequence were characters that fit in a string. See [Input Events](Input-Events.html).

        (this-command-keys)
        ;; Now use C-u C-x C-e to evaluate that.
             ⇒ "^U^X^E"

<!---->

*   Function: **this-command-keys-vector**

    Like `this-command-keys`, except that it always returns the events in a vector, so you don’t need to deal with the complexities of storing input events in a string (see [Strings of Events](Strings-of-Events.html)).

<!---->

*   Function: **clear-this-command-keys** *\&optional keep-record*

    This function empties out the table of events for `this-command-keys` to return. Unless `keep-record` is non-`nil`, it also empties the records that the function `recent-keys` (see [Recording Input](Recording-Input.html)) will subsequently return. This is useful after reading a password, to prevent the password from echoing inadvertently as part of the next command in certain cases.

<!---->

*   Variable: **last-nonmenu-event**

    This variable holds the last input event read as part of a key sequence, not counting events resulting from mouse menus.

    One use of this variable is for telling `x-popup-menu` where to pop up a menu. It is also used internally by `y-or-n-p` (see [Yes-or-No Queries](Yes_002dor_002dNo-Queries.html)).

<!---->

*   Variable: **last-command-event**

    This variable is set to the last input event that was read by the command loop as part of a command. The principal use of this variable is in `self-insert-command`, which uses it to decide which character to insert.

        last-command-event
        ;; Now use C-u C-x C-e to evaluate that.
             ⇒ 5

    The value is 5 because that is the ASCII code for `C-e`.

<!---->

*   Variable: **last-event-frame**

    This variable records which frame the last input event was directed to. Usually this is the frame that was selected when the event was generated, but if that frame has redirected input focus to another frame, the value is the frame to which the event was redirected. See [Input Focus](Input-Focus.html).

    If the last event came from a keyboard macro, the value is `macro`.

Next: [Adjusting Point](Adjusting-Point.html), Previous: [Distinguish Interactive](Distinguish-Interactive.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
