

Next: [Quitting](Quitting.html), Previous: [Special Events](Special-Events.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 21.10 Waiting for Elapsed Time or Input

The wait functions are designed to wait for a certain amount of time to pass or until there is input. For example, you may wish to pause in the middle of a computation to allow the user time to view the display. `sit-for` pauses and updates the screen, and returns immediately if input comes in, while `sleep-for` pauses without updating the screen.

*   Function: **sit-for** *seconds \&optional nodisp*

    This function performs redisplay (provided there is no pending input from the user), then waits `seconds` seconds, or until input is available. The usual purpose of `sit-for` is to give the user time to read text that you display. The value is `t` if `sit-for` waited the full time with no input arriving (see [Event Input Misc](Event-Input-Misc.html)). Otherwise, the value is `nil`.

    The argument `seconds` need not be an integer. If it is floating point, `sit-for` waits for a fractional number of seconds. Some systems support only a whole number of seconds; on these systems, `seconds` is rounded down.

    The expression `(sit-for 0)` is equivalent to `(redisplay)`, i.e., it requests a redisplay, without any delay, if there is no pending input. See [Forcing Redisplay](Forcing-Redisplay.html).

    If `nodisp` is non-`nil`, then `sit-for` does not redisplay, but it still returns as soon as input is available (or when the timeout elapses).

    In batch mode (see [Batch Mode](Batch-Mode.html)), `sit-for` cannot be interrupted, even by input from the standard input descriptor. It is thus equivalent to `sleep-for`, which is described below.

    It is also possible to call `sit-for` with three arguments, as `(sit-for seconds millisec nodisp)`, but that is considered obsolete.

<!---->

*   Function: **sleep-for** *seconds \&optional millisec*

    This function simply pauses for `seconds` seconds without updating the display. It pays no attention to available input. It returns `nil`.

    The argument `seconds` need not be an integer. If it is floating point, `sleep-for` waits for a fractional number of seconds. Some systems support only a whole number of seconds; on these systems, `seconds` is rounded down.

    The optional argument `millisec` specifies an additional waiting period measured in milliseconds. This adds to the period specified by `seconds`. If the system doesn’t support waiting fractions of a second, you get an error if you specify nonzero `millisec`.

    Use `sleep-for` when you wish to guarantee a delay.

See [Time of Day](Time-of-Day.html), for functions to get the current time.

Next: [Quitting](Quitting.html), Previous: [Special Events](Special-Events.html), Up: [Command Loop](Command-Loop.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
