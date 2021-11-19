

Next: [Edebug Misc](Edebug-Misc.html), Previous: [Edebug Execution Modes](Edebug-Execution-Modes.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 18.2.4 Jumping

The commands described in this section execute until they reach a specified location. All except `i` make a temporary breakpoint to establish the place to stop, then switch to go mode. Any other breakpoint reached before the intended stop point will also stop execution. See [Breakpoints](Breakpoints.html), for the details on breakpoints.

These commands may fail to work as expected in case of nonlocal exit, as that can bypass the temporary breakpoint where you expected the program to stop.

*   `h`

    Proceed to the stop point near where point is (`edebug-goto-here`).

*   `f`

    Run the program for one expression (`edebug-forward-sexp`).

*   `o`

    Run the program until the end of the containing sexp (`edebug-step-out`).

*   `i`

    Step into the function or macro called by the form after point (`edebug-step-in`).

The `h` command proceeds to the stop point at or after the current location of point, using a temporary breakpoint.

The `f` command runs the program forward over one expression. More precisely, it sets a temporary breakpoint at the position that `forward-sexp` would reach, then executes in go mode so that the program will stop at breakpoints.

With a prefix argument `n`, the temporary breakpoint is placed `n` sexps beyond point. If the containing list ends before `n` more elements, then the place to stop is after the containing expression.

You must check that the position `forward-sexp` finds is a place that the program will really get to. In `cond`, for example, this may not be true.

For flexibility, the `f` command does `forward-sexp` starting at point, rather than at the stop point. If you want to execute one expression *from the current stop point*, first type `w` (`edebug-where`) to move point there, and then type `f`.

The `o` command continues out of an expression. It places a temporary breakpoint at the end of the sexp containing point. If the containing sexp is a function definition itself, `o` continues until just before the last sexp in the definition. If that is where you are now, it returns from the function and then stops. In other words, this command does not exit the currently executing function unless you are positioned after the last sexp.

Normally, the `h`, `f`, and `o` commands display “Break” and pause for `edebug-sit-for-seconds` before showing the result of the form just evaluated. You can avoid this pause by setting `edebug-sit-on-break` to `nil`. See [Edebug Options](Edebug-Options.html).

The `i` command steps into the function or macro called by the list form after point, and stops at its first stop point. Note that the form need not be the one about to be evaluated. But if the form is a function call about to be evaluated, remember to use this command before any of the arguments are evaluated, since otherwise it will be too late.

The `i` command instruments the function or macro it’s supposed to step into, if it isn’t instrumented already. This is convenient, but keep in mind that the function or macro remains instrumented unless you explicitly arrange to deinstrument it.

Next: [Edebug Misc](Edebug-Misc.html), Previous: [Edebug Execution Modes](Edebug-Execution-Modes.html), Up: [Edebug](Edebug.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
