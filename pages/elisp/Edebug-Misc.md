

#### 18.2.5 Miscellaneous Edebug Commands

Some miscellaneous Edebug commands are described here.

`?`

Display the help message for Edebug (`edebug-help`).

*   `a`
*   `C-]`

Abort one level back to the previous command level (`abort-recursive-edit`).

`q`

Return to the top level editor command loop (`top-level`). This exits all recursive editing levels, including all levels of Edebug activity. However, instrumented code protected with `unwind-protect` or `condition-case` forms may resume debugging.

`Q`

Like `q`, but don’t stop even for protected code (`edebug-top-level-nonstop`).

`r`

Redisplay the most recently known expression result in the echo area (`edebug-previous-result`).

`d`

Display a backtrace, excluding Edebug’s own functions for clarity (`edebug-pop-to-backtrace`).

See [Backtraces](Backtraces.html), for a description of backtraces and the commands which work on them.

If you would like to see Edebug’s functions in the backtrace, use `M-x edebug-backtrace-show-instrumentation`. To hide them again use `M-x edebug-backtrace-hide-instrumentation`.

If a backtrace frame starts with ‘`>`’ that means that Edebug knows where the source code for the frame is located. Use `s` to jump to the source code for the current frame.

The backtrace buffer is killed automatically when you continue execution.

You can invoke commands from Edebug that activate Edebug again recursively. Whenever Edebug is active, you can quit to the top level with `q` or abort one recursive edit level with `C-]`. You can display a backtrace of all the pending evaluations with `d`.
