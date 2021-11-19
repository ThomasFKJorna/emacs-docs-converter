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

Next: [Query Before Exit](Query-Before-Exit.html), Previous: [Output from Processes](Output-from-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.10 Sentinels: Detecting Process Status Changes

A *process sentinel* is a function that is called whenever the associated process changes status for any reason, including signals (whether sent by Emacs or caused by the process’s own actions) that terminate, stop, or continue the process. The process sentinel is also called if the process exits. The sentinel receives two arguments: the process for which the event occurred, and a string describing the type of event.

If no sentinel function was specified for a process, it will use the default sentinel function, which inserts a message in the process’s buffer with the process name and the string describing the event.

The string describing the event looks like one of the following (but this is not an exhaustive list of event strings):

*   `"finished\n"`.
*   `"deleted\n"`.
*   `"exited abnormally with code exitcode (core dumped)\n"`. The “core dumped” part is optional, and only appears if the process dumped core.
*   `"failed with code fail-code\n"`.
*   `"signal-description (core dumped)\n"`. The `signal-description` is a system-dependent textual description of a signal, e.g., `"killed"` for `SIGKILL`. The “core dumped” part is optional, and only appears if the process dumped core.
*   `"open from host-name\n"`.
*   `"open\n"`.
*   `"run\n"`.
*   `"connection broken by remote peer\n"`.

A sentinel runs only while Emacs is waiting (e.g., for terminal input, or for time to elapse, or for process output). This avoids the timing errors that could result from running sentinels at random places in the middle of other Lisp programs. A program can wait, so that sentinels will run, by calling `sit-for` or `sleep-for` (see [Waiting](Waiting.html)), or `accept-process-output` (see [Accepting Output](Accepting-Output.html)). Emacs also allows sentinels to run when the command loop is reading input. `delete-process` calls the sentinel when it terminates a running process.

Emacs does not keep a queue of multiple reasons to call the sentinel of one process; it records just the current status and the fact that there has been a change. Therefore two changes in status, coming in quick succession, can call the sentinel just once. However, process termination will always run the sentinel exactly once. This is because the process status can’t change again after termination.

Emacs explicitly checks for output from the process before running the process sentinel. Once the sentinel runs due to process termination, no further output can arrive from the process.

A sentinel that writes the output into the buffer of the process should check whether the buffer is still alive. If it tries to insert into a dead buffer, it will get an error. If the buffer is dead, `(buffer-name (process-buffer process))` returns `nil`.

Quitting is normally inhibited within a sentinel—otherwise, the effect of typing `C-g` at command level or to quit a user command would be unpredictable. If you want to permit quitting inside a sentinel, bind `inhibit-quit` to `nil`. In most cases, the right way to do this is with the macro `with-local-quit`. See [Quitting](Quitting.html).

If an error happens during execution of a sentinel, it is caught automatically, so that it doesn’t stop the execution of whatever programs was running when the sentinel was started. However, if `debug-on-error` is non-`nil`, errors are not caught. This makes it possible to use the Lisp debugger to debug the sentinel. See [Debugger](Debugger.html).

While a sentinel is running, the process sentinel is temporarily set to `nil` so that the sentinel won’t run recursively. For this reason it is not possible for a sentinel to specify a new sentinel.

Note that Emacs automatically saves and restores the match data while executing sentinels. See [Match Data](Match-Data.html).

*   Function: **set-process-sentinel** *process sentinel*

    This function associates `sentinel` with `process`. If `sentinel` is `nil`, then the process will have the default sentinel, which inserts a message in the process’s buffer when the process status changes.

    Changes in process sentinels take effect immediately—if the sentinel is slated to be run but has not been called yet, and you specify a new sentinel, the eventual call to the sentinel will use the new one.

        (defun msg-me (process event)
           (princ
             (format "Process: %s had the event '%s'" process event)))
        (set-process-sentinel (get-process "shell") 'msg-me)
             ⇒ msg-me

    <!---->

        (kill-process (get-process "shell"))
             -| Process: #<process shell> had the event 'killed'
             ⇒ #<process shell>

<!---->

*   Function: **process-sentinel** *process*

    This function returns the sentinel of `process`.

In case a process status changes need to be passed to several sentinels, you can use `add-function` to combine an existing sentinel with a new one. See [Advising Functions](Advising-Functions.html).

*   Function: **waiting-for-user-input-p**

    While a sentinel or filter function is running, this function returns non-`nil` if Emacs was waiting for keyboard input from the user at the time the sentinel or filter function was called, or `nil` if it was not.

Next: [Query Before Exit](Query-Before-Exit.html), Previous: [Output from Processes](Output-from-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
