

Next: [Variable Debugging](Variable-Debugging.html), Previous: [Infinite Loops](Infinite-Loops.html), Up: [Debugger](Debugger.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.1.3 Entering the Debugger on a Function Call

To investigate a problem that happens in the middle of a program, one useful technique is to enter the debugger whenever a certain function is called. You can do this to the function in which the problem occurs, and then step through the function, or you can do this to a function called shortly before the problem, step quickly over the call to that function, and then step through its caller.

*   Command: **debug-on-entry** *function-name*

    This function requests `function-name` to invoke the debugger each time it is called.

    Any function or macro defined as Lisp code may be set to break on entry, regardless of whether it is interpreted code or compiled code. If the function is a command, it will enter the debugger when called from Lisp and when called interactively (after the reading of the arguments). You can also set debug-on-entry for primitive functions (i.e., those written in C) this way, but it only takes effect when the primitive is called from Lisp code. Debug-on-entry is not allowed for special forms.

    When `debug-on-entry` is called interactively, it prompts for `function-name` in the minibuffer. If the function is already set up to invoke the debugger on entry, `debug-on-entry` does nothing. `debug-on-entry` always returns `function-name`.

    Here’s an example to illustrate use of this function:

    ```lisp
    (defun fact (n)
      (if (zerop n) 1
          (* n (fact (1- n)))))
         ⇒ fact
    ```

    ```lisp
    (debug-on-entry 'fact)
         ⇒ fact
    ```

    ```lisp
    (fact 3)
    ```

    ```lisp
    ```

    ```lisp
    ------ Buffer: *Backtrace* ------
    Debugger entered--entering a function:
    * fact(3)
      eval((fact 3))
      eval-last-sexp-1(nil)
      eval-last-sexp(nil)
      call-interactively(eval-last-sexp)
    ------ Buffer: *Backtrace* ------
    ```

    ```lisp
    ```

<!---->

*   Command: **cancel-debug-on-entry** *\&optional function-name*

    This function undoes the effect of `debug-on-entry` on `function-name`. When called interactively, it prompts for `function-name` in the minibuffer. If `function-name` is omitted or `nil`, it cancels break-on-entry for all functions. Calling `cancel-debug-on-entry` does nothing to a function which is not currently set up to break on entry.

Next: [Variable Debugging](Variable-Debugging.html), Previous: [Infinite Loops](Infinite-Loops.html), Up: [Debugger](Debugger.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
