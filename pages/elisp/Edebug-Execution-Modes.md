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

Next: [Jumping](Jumping.html), Previous: [Instrumenting](Instrumenting.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.3 Edebug Execution Modes

Edebug supports several execution modes for running the program you are debugging. We call these alternatives *Edebug execution modes*; do not confuse them with major or minor modes. The current Edebug execution mode determines how far Edebug continues execution before stopping—whether it stops at each stop point, or continues to the next breakpoint, for example—and how much Edebug displays the progress of the evaluation before it stops.

Normally, you specify the Edebug execution mode by typing a command to continue the program in a certain mode. Here is a table of these commands; all except for `S` resume execution of the program, at least for a certain distance.

*   `S`

    Stop: don’t execute any more of the program, but wait for more Edebug commands (`edebug-stop`).

*   `SPC`

    Step: stop at the next stop point encountered (`edebug-step-mode`).

*   `n`

    Next: stop at the next stop point encountered after an expression (`edebug-next-mode`). Also see `edebug-forward-sexp` in [Jumping](Jumping.html).

*   `t`

    Trace: pause (normally one second) at each Edebug stop point (`edebug-trace-mode`).

*   `T`

    Rapid trace: update the display at each stop point, but don’t actually pause (`edebug-Trace-fast-mode`).

*   `g`

    Go: run until the next breakpoint (`edebug-go-mode`). See [Breakpoints](Breakpoints.html).

*   `c`

    Continue: pause one second at each breakpoint, and then continue (`edebug-continue-mode`).

*   `C`

    Rapid continue: move point to each breakpoint, but don’t pause (`edebug-Continue-fast-mode`).

*   `G`

    Go non-stop: ignore breakpoints (`edebug-Go-nonstop-mode`). You can still stop the program by typing `S`, or any editing command.

In general, the execution modes earlier in the above list run the program more slowly or stop sooner than the modes later in the list.

When you enter a new Edebug level, Edebug will normally stop at the first instrumented function it encounters. If you prefer to stop only at a break point, or not at all (for example, when gathering coverage data), change the value of `edebug-initial-mode` from its default `step` to `go`, or `Go-nonstop`, or one of its other values (see [Edebug Options](Edebug-Options.html)). You can do this readily with `C-x C-a C-m` (`edebug-set-initial-mode`):

*   Command: **edebug-set-initial-mode**

    This command, bound to `C-x C-a C-m`, sets `edebug-initial-mode`. It prompts you for a key to indicate the mode. You should enter one of the eight keys listed above, which sets the corresponding mode.

Note that you may reenter the same Edebug level several times if, for example, an instrumented function is called several times from one command.

While executing or tracing, you can interrupt the execution by typing any Edebug command. Edebug stops the program at the next stop point and then executes the command you typed. For example, typing `t` during execution switches to trace mode at the next stop point. You can use `S` to stop execution without doing anything else.

If your function happens to read input, a character you type intending to interrupt execution may be read by the function instead. You can avoid such unintended results by paying attention to when your program wants input.

Keyboard macros containing the commands in this section do not completely work: exiting from Edebug, to resume the program, loses track of the keyboard macro. This is not easy to fix. Also, defining or executing a keyboard macro outside of Edebug does not affect commands inside Edebug. This is usually an advantage. See also the `edebug-continue-kbd-macro` option in [Edebug Options](Edebug-Options.html).

*   User Option: **edebug-sit-for-seconds**

    This option specifies how many seconds to wait between execution steps in trace mode or continue mode. The default is 1 second.

Next: [Jumping](Jumping.html), Previous: [Instrumenting](Instrumenting.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
