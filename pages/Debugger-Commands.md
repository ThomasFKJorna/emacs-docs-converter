

Next: [Invoking the Debugger](Invoking-the-Debugger.html), Previous: [Backtraces](Backtraces.html), Up: [Debugger](Debugger.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.1.8 Debugger Commands

The debugger buffer (in Debugger mode) provides special commands in addition to the usual Emacs commands and to the Backtrace mode commands described in the previous section. The most important use of debugger commands is for stepping through code, so that you can see how control flows. The debugger can step through the control structures of an interpreted function, but cannot do so in a byte-compiled function. If you would like to step through a byte-compiled function, replace it with an interpreted definition of the same function. (To do this, visit the source for the function and type `C-M-x` on its definition.) You cannot use the Lisp debugger to step through a primitive function.

Some of the debugger commands operate on the current frame. If a frame starts with a star, that means that exiting that frame will call the debugger again. This is useful for examining the return value of a function.

Here is a list of Debugger mode commands:

*   `c`

    Exit the debugger and continue execution. This resumes execution of the program as if the debugger had never been entered (aside from any side-effects that you caused by changing variable values or data structures while inside the debugger).

*   `d`

    Continue execution, but enter the debugger the next time any Lisp function is called. This allows you to step through the subexpressions of an expression, seeing what values the subexpressions compute, and what else they do.

    The stack frame made for the function call which enters the debugger in this way will be flagged automatically so that the debugger will be called again when the frame is exited. You can use the `u` command to cancel this flag.

*   `b`

    Flag the current frame so that the debugger will be entered when the frame is exited. Frames flagged in this way are marked with stars in the backtrace buffer.

*   `u`

    Don’t enter the debugger when the current frame is exited. This cancels a `b` command on that frame. The visible effect is to remove the star from the line in the backtrace buffer.

*   `j`

    Flag the current frame like `b`. Then continue execution like `c`, but temporarily disable break-on-entry for all functions that are set up to do so by `debug-on-entry`.

*   `e`

    Read a Lisp expression in the minibuffer, evaluate it (with the relevant lexical environment, if applicable), and print the value in the echo area. The debugger alters certain important variables, and the current buffer, as part of its operation; `e` temporarily restores their values from outside the debugger, so you can examine and change them. This makes the debugger more transparent. By contrast, `M-:` does nothing special in the debugger; it shows you the variable values within the debugger.

*   `R`

    Like `e`, but also save the result of evaluation in the buffer `*Debugger-record*`.

*   `q`

    Terminate the program being debugged; return to top-level Emacs command execution.

    If the debugger was entered due to a `C-g` but you really want to quit, and not debug, use the `q` command.

*   `r`

    Return a value from the debugger. The value is computed by reading an expression with the minibuffer and evaluating it.

    The `r` command is useful when the debugger was invoked due to exit from a Lisp call frame (as requested with `b` or by entering the frame with `d`); then the value specified in the `r` command is used as the value of that frame. It is also useful if you call `debug` and use its return value. Otherwise, `r` has the same effect as `c`, and the specified return value does not matter.

    You can’t use `r` when the debugger was entered due to an error.

*   `l`

    Display a list of functions that will invoke the debugger when called. This is a list of functions that are set to break on entry by means of `debug-on-entry`.

Next: [Invoking the Debugger](Invoking-the-Debugger.html), Previous: [Backtraces](Backtraces.html), Up: [Debugger](Debugger.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
