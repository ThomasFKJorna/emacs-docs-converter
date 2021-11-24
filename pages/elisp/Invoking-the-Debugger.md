

#### 18.1.9 Invoking the Debugger

Here we describe in full detail the function `debug` that is used to invoke the debugger.

### Command: **debug** *\&rest debugger-args*

This function enters the debugger. It switches buffers to a buffer named `*Backtrace*` (or `*Backtrace*<2>` if it is the second recursive entry to the debugger, etc.), and fills it with information about the stack of Lisp function calls. It then enters a recursive edit, showing the backtrace buffer in Debugger mode.

The Debugger mode `c`, `d`, `j`, and `r` commands exit the recursive edit; then `debug` switches back to the previous buffer and returns to whatever called `debug`. This is the only way the function `debug` can return to its caller.

The use of the `debugger-args` is that `debug` displays the rest of its arguments at the top of the `*Backtrace*` buffer, so that the user can see them. Except as described below, this is the *only* way these arguments are used.

However, certain values for first argument to `debug` have a special significance. (Normally, these values are used only by the internals of Emacs, and not by programmers calling `debug`.) Here is a table of these special values:

*   `lambda`

    A first argument of `lambda` means `debug` was called because of entry to a function when `debug-on-next-call` was non-`nil`. The debugger displays ‘`Debugger entered--entering a function:`’ as a line of text at the top of the buffer.

*   `debug`

    `debug` as first argument means `debug` was called because of entry to a function that was set to debug on entry. The debugger displays the string ‘`Debugger entered--entering a function:`’, just as in the `lambda` case. It also marks the stack frame for that function so that it will invoke the debugger when exited.

*   `t`

    When the first argument is `t`, this indicates a call to `debug` due to evaluation of a function call form when `debug-on-next-call` is non-`nil`. The debugger displays ‘`Debugger entered--beginning evaluation of function call form:`’ as the top line in the buffer.

*   `exit`

    When the first argument is `exit`, it indicates the exit of a stack frame previously marked to invoke the debugger on exit. The second argument given to `debug` in this case is the value being returned from the frame. The debugger displays ‘`Debugger entered--returning value:`’ in the top line of the buffer, followed by the value being returned.

*   `error`

    When the first argument is `error`, the debugger indicates that it is being entered because an error or `quit` was signaled and not handled, by displaying ‘`Debugger entered--Lisp error:`’ followed by the error signaled and any arguments to `signal`. For example,

    ```lisp
    (let ((debug-on-error t))
      (/ 1 0))
    ```

    ```lisp
    ```

    ```lisp
    ------ Buffer: *Backtrace* ------
    Debugger entered--Lisp error: (arith-error)
      /(1 0)
    ...
    ------ Buffer: *Backtrace* ------
    ```

    If an error was signaled, presumably the variable `debug-on-error` is non-`nil`. If `quit` was signaled, then presumably the variable `debug-on-quit` is non-`nil`.

*   `nil`

    Use `nil` as the first of the `debugger-args` when you want to enter the debugger explicitly. The rest of the `debugger-args` are printed on the top line of the buffer. You can use this feature to display messages—for example, to remind yourself of the conditions under which `debug` is called.
