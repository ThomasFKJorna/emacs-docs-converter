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

Next: [Output from Processes](Output-from-Processes.html), Previous: [Input to Processes](Input-to-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 38.8 Sending Signals to Processes

*Sending a signal* to a subprocess is a way of interrupting its activities. There are several different signals, each with its own meaning. The set of signals and their names is defined by the operating system. For example, the signal `SIGINT` means that the user has typed `C-c`, or that some analogous thing has happened.

Each signal has a standard effect on the subprocess. Most signals kill the subprocess, but some stop (or resume) execution instead. Most signals can optionally be handled by programs; if the program handles the signal, then we can say nothing in general about its effects.

You can send signals explicitly by calling the functions in this section. Emacs also sends signals automatically at certain times: killing a buffer sends a `SIGHUP` signal to all its associated processes; killing Emacs sends a `SIGHUP` signal to all remaining processes. (`SIGHUP` is a signal that usually indicates that the user “hung up the phone”, i.e., disconnected.)

Each of the signal-sending functions takes two optional arguments: `process` and `current-group`.

The argument `process` must be either a process, a process name, a buffer, a buffer name, or `nil`. A buffer or buffer name stands for a process through `get-buffer-process`. `nil` stands for the process associated with the current buffer. Except with `stop-process` and `continue-process`, an error is signaled if `process` does not identify an active process, or if it represents a network, serial, or pipe connection.

The argument `current-group` is a flag that makes a difference when you are running a job-control shell as an Emacs subprocess. If it is non-`nil`, then the signal is sent to the current process-group of the terminal that Emacs uses to communicate with the subprocess. If the process is a job-control shell, this means the shell’s current subjob. If `current-group` is `nil`, the signal is sent to the process group of the immediate subprocess of Emacs. If the subprocess is a job-control shell, this is the shell itself. If `current-group` is `lambda`, the signal is sent to the process-group that owns the terminal, but only if it is not the shell itself.

The flag `current-group` has no effect when a pipe is used to communicate with the subprocess, because the operating system does not support the distinction in the case of pipes. For the same reason, job-control shells won’t work when a pipe is used. See `process-connection-type` in [Asynchronous Processes](Asynchronous-Processes.html).

*   Function: **interrupt-process** *\&optional process current-group*

    This function interrupts the process `process` by sending the signal `SIGINT`. Outside of Emacs, typing the interrupt character (normally `C-c` on some systems, and `DEL` on others) sends this signal. When the argument `current-group` is non-`nil`, you can think of this function as typing `C-c` on the terminal by which Emacs talks to the subprocess.

<!---->

*   Function: **kill-process** *\&optional process current-group*

    This function kills the process `process` by sending the signal `SIGKILL`. This signal kills the subprocess immediately, and cannot be handled by the subprocess.

<!---->

*   Function: **quit-process** *\&optional process current-group*

    This function sends the signal `SIGQUIT` to the process `process`. This signal is the one sent by the quit character (usually `C-\`) when you are not inside Emacs.

<!---->

*   Function: **stop-process** *\&optional process current-group*

    This function stops the specified `process`. If it is a real subprocess running a program, it sends the signal `SIGTSTP` to that subprocess. If `process` represents a network, serial, or pipe connection, this function inhibits handling of the incoming data from the connection; for a network server, this means not accepting new connections. Use `continue-process` to resume normal execution.

    Outside of Emacs, on systems with job control, the stop character (usually `C-z`) normally sends the `SIGTSTP` signal to a subprocess. When `current-group` is non-`nil`, you can think of this function as typing `C-z` on the terminal Emacs uses to communicate with the subprocess.

<!---->

*   Function: **continue-process** *\&optional process current-group*

    This function resumes execution of the process `process`. If it is a real subprocess running a program, it sends the signal `SIGCONT` to that subprocess; this presumes that `process` was stopped previously. If `process` represents a network, serial, or pipe connection, this function resumes handling of the incoming data from the connection. For serial connections, data that arrived during the time the process was stopped might be lost.

<!---->

*   Command: **signal-process** *process signal*

    This function sends a signal to process `process`. The argument `signal` specifies which signal to send; it should be an integer, or a symbol whose name is a signal.

    The `process` argument can be a system process ID (an integer); that allows you to send signals to processes that are not children of Emacs. See [System Processes](System-Processes.html).

Sometimes, it is necessary to send a signal to a non-local asynchronous process. This is possible by writing an own `interrupt-process` implementation. This function must be added then to `interrupt-process-functions`.

*   Variable: **interrupt-process-functions**

    This variable is a list of functions to be called for `interrupt-process`. The arguments of the functions are the same as for `interrupt-process`. These functions are called in the order of the list, until one of them returns non-`nil`. The default function, which shall always be the last in this list, is `internal-default-interrupt-process`.

    This is the mechanism, how Tramp implements `interrupt-process`.

Next: [Output from Processes](Output-from-Processes.html), Previous: [Input to Processes](Input-to-Processes.html), Up: [Processes](Processes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
