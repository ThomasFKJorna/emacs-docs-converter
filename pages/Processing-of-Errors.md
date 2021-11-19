

Next: [Handling Errors](Handling-Errors.html), Previous: [Signaling Errors](Signaling-Errors.html), Up: [Errors](Errors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 11.7.3.2 How Emacs Processes Errors

When an error is signaled, `signal` searches for an active *handler* for the error. A handler is a sequence of Lisp expressions designated to be executed if an error happens in part of the Lisp program. If the error has an applicable handler, the handler is executed, and control resumes following the handler. The handler executes in the environment of the `condition-case` that established it; all functions called within that `condition-case` have already been exited, and the handler cannot return to them.

If there is no applicable handler for the error, it terminates the current command and returns control to the editor command loop. (The command loop has an implicit handler for all kinds of errors.) The command loop’s handler uses the error symbol and associated data to print an error message. You can use the variable `command-error-function` to control how this is done:

*   Variable: **command-error-function**

    This variable, if non-`nil`, specifies a function to use to handle errors that return control to the Emacs command loop. The function should take three arguments: `data`, a list of the same form that `condition-case` would bind to its variable; `context`, a string describing the situation in which the error occurred, or (more often) `nil`; and `caller`, the Lisp function which called the primitive that signaled the error.

An error that has no explicit handler may call the Lisp debugger. The debugger is enabled if the variable `debug-on-error` (see [Error Debugging](Error-Debugging.html)) is non-`nil`. Unlike error handlers, the debugger runs in the environment of the error, so that you can examine values of variables precisely as they were at the time of the error.

Next: [Handling Errors](Handling-Errors.html), Previous: [Signaling Errors](Signaling-Errors.html), Up: [Errors](Errors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
