

Next: [Inline Functions](Inline-Functions.html), Previous: [Advising Functions](Advising-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 13.12 Declaring Functions Obsolete

You can mark a named function as *obsolete*, meaning that it may be removed at some point in the future. This causes Emacs to warn that the function is obsolete whenever it byte-compiles code containing that function, and whenever it displays the documentation for that function. In all other respects, an obsolete function behaves like any other function.

The easiest way to mark a function as obsolete is to put a `(declare (obsolete …))` form in the function’s `defun` definition. See [Declare Form](Declare-Form.html). Alternatively, you can use the `make-obsolete` function, described below.

A macro (see [Macros](Macros.html)) can also be marked obsolete with `make-obsolete`; this has the same effects as for a function. An alias for a function or macro can also be marked as obsolete; this makes the alias itself obsolete, not the function or macro which it resolves to.

*   Function: **make-obsolete** *obsolete-name current-name when*

    This function marks `obsolete-name` as obsolete. `obsolete-name` should be a symbol naming a function or macro, or an alias for a function or macro.

    If `current-name` is a symbol, the warning message says to use `current-name` instead of `obsolete-name`. `current-name` does not need to be an alias for `obsolete-name`; it can be a different function with similar functionality. `current-name` can also be a string, which serves as the warning message. The message should begin in lower case, and end with a period. It can also be `nil`, in which case the warning message provides no additional details.

    The argument `when` should be a string indicating when the function was first made obsolete—for example, a date or a release number.

<!---->

*   Macro: **define-obsolete-function-alias** *obsolete-name current-name when \&optional doc*

    This convenience macro marks the function `obsolete-name` obsolete and also defines it as an alias for the function `current-name`. It is equivalent to the following:

    ```lisp
    (defalias obsolete-name current-name doc)
    (make-obsolete obsolete-name current-name when)
    ```

In addition, you can mark a particular calling convention for a function as obsolete:

*   Function: **set-advertised-calling-convention** *function signature when*

    This function specifies the argument list `signature` as the correct way to call `function`. This causes the Emacs byte compiler to issue a warning whenever it comes across an Emacs Lisp program that calls `function` any other way (however, it will still allow the code to be byte compiled). `when` should be a string indicating when the variable was first made obsolete (usually a version number string).

    For instance, in old versions of Emacs the `sit-for` function accepted three arguments, like this

    ```lisp
      (sit-for seconds milliseconds nodisp)
    ```

    However, calling `sit-for` this way is considered obsolete (see [Waiting](Waiting.html)). The old calling convention is deprecated like this:

    ```lisp
    (set-advertised-calling-convention
      'sit-for '(seconds &optional nodisp) "22.1")
    ```

Next: [Inline Functions](Inline-Functions.html), Previous: [Advising Functions](Advising-Functions.html), Up: [Functions](Functions.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
