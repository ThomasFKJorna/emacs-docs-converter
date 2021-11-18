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

Next: [Idle Timers](Idle-Timers.html), Previous: [Time Calculations](Time-Calculations.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.11 Timers for Delayed Execution

You can set up a *timer* to call a function at a specified future time or after a certain length of idleness. A timer is a special object that stores the information about the next invocation times and the function to invoke.

*   Function: **timerp** *object*

    This predicate function returns non-`nil` if `object` is a timer.

Emacs cannot run timers at any arbitrary point in a Lisp program; it can run them only when Emacs could accept output from a subprocess: namely, while waiting or inside certain primitive functions such as `sit-for` or `read-event` which *can* wait. Therefore, a timer’s execution may be delayed if Emacs is busy. However, the time of execution is very precise if Emacs is idle.

Emacs binds `inhibit-quit` to `t` before calling the timer function, because quitting out of many timer functions can leave things in an inconsistent state. This is normally unproblematical because most timer functions don’t do a lot of work. Indeed, for a timer to call a function that takes substantial time to run is likely to be annoying. If a timer function needs to allow quitting, it should use `with-local-quit` (see [Quitting](Quitting.html)). For example, if a timer function calls `accept-process-output` to receive output from an external process, that call should be wrapped inside `with-local-quit`, to ensure that `C-g` works if the external process hangs.

It is usually a bad idea for timer functions to alter buffer contents. When they do, they usually should call `undo-boundary` both before and after changing the buffer, to separate the timer’s changes from user commands’ changes and prevent a single undo entry from growing to be quite large.

Timer functions should also avoid calling functions that cause Emacs to wait, such as `sit-for` (see [Waiting](Waiting.html)). This can lead to unpredictable effects, since other timers (or even the same timer) can run while waiting. If a timer function needs to perform an action after a certain time has elapsed, it can do this by scheduling a new timer.

If a timer function calls functions that can change the match data, it should save and restore the match data. See [Saving Match Data](Saving-Match-Data.html).

*   Command: **run-at-time** *time repeat function \&rest args*

    This sets up a timer that calls the function `function` with arguments `args` at time `time`. If `repeat` is a number (integer or floating point), the timer is scheduled to run again every `repeat` seconds after `time`. If `repeat` is `nil`, the timer runs only once.

    `time` may specify an absolute or a relative time.

    Absolute times may be specified using a string with a limited variety of formats, and are taken to be times *today*, even if already in the past. The recognized forms are ‘`xxxx`’, ‘`x:xx`’, or ‘`xx:xx`’ (military time), and ‘`xxam`’, ‘`xxAM`’, ‘`xxpm`’, ‘`xxPM`’, ‘`xx:xxam`’, ‘`xx:xxAM`’, ‘`xx:xxpm`’, or ‘`xx:xxPM`’. A period can be used instead of a colon to separate the hour and minute parts.

    To specify a relative time as a string, use numbers followed by units. For example:

    *   ‘`1 min`’

        denotes 1 minute from now.

    *   ‘`1 min 5 sec`’

        denotes 65 seconds from now.

    *   ‘`1 min 2 sec 3 hour 4 day 5 week 6 fortnight 7 month 8 year`’

        denotes exactly 103 months, 123 days, and 10862 seconds from now.

    For relative time values, Emacs considers a month to be exactly thirty days, and a year to be exactly 365.25 days.

    Not all convenient formats are strings. If `time` is a number (integer or floating point), that specifies a relative time measured in seconds. The result of `encode-time` can also be used to specify an absolute value for `time`.

    In most cases, `repeat` has no effect on when *first* call takes place—`time` alone specifies that. There is one exception: if `time` is `t`, then the timer runs whenever the time is a multiple of `repeat` seconds after the epoch. This is useful for functions like `display-time`.

    The function `run-at-time` returns a timer value that identifies the particular scheduled future action. You can use this value to call `cancel-timer` (see below).

<!---->

*   Command: **run-with-timer** *secs repeat function \&rest args*

    This is exactly the same as `run-at-time` (so see that definition for an explanation of the parameters; `secs` is passed as `time` to that function), but is meant to be used when the delay is specified in seconds.

A repeating timer nominally ought to run every `repeat` seconds, but remember that any invocation of a timer can be late. Lateness of one repetition has no effect on the scheduled time of the next repetition. For instance, if Emacs is busy computing for long enough to cover three scheduled repetitions of the timer, and then starts to wait, it will immediately call the timer function three times in immediate succession (presuming no other timers trigger before or between them). If you want a timer to run again no less than `n` seconds after the last invocation, don’t use the `repeat` argument. Instead, the timer function should explicitly reschedule the timer.

*   User Option: **timer-max-repeats**

    This variable’s value specifies the maximum number of times to repeat calling a timer function in a row, when many previously scheduled calls were unavoidably delayed.

<!---->

*   Macro: **with-timeout** *(seconds timeout-forms…) body…*

    Execute `body`, but give up after `seconds` seconds. If `body` finishes before the time is up, `with-timeout` returns the value of the last form in `body`. If, however, the execution of `body` is cut short by the timeout, then `with-timeout` executes all the `timeout-forms` and returns the value of the last of them.

    This macro works by setting a timer to run after `seconds` seconds. If `body` finishes before that time, it cancels the timer. If the timer actually runs, it terminates execution of `body`, then executes `timeout-forms`.

    Since timers can run within a Lisp program only when the program calls a primitive that can wait, `with-timeout` cannot stop executing `body` while it is in the midst of a computation—only when it calls one of those primitives. So use `with-timeout` only with a `body` that waits for input, not one that does a long computation.

The function `y-or-n-p-with-timeout` provides a simple way to use a timer to avoid waiting too long for an answer. See [Yes-or-No Queries](Yes_002dor_002dNo-Queries.html).

*   Function: **cancel-timer** *timer*

    This cancels the requested action for `timer`, which should be a timer—usually, one previously returned by `run-at-time` or `run-with-idle-timer`. This cancels the effect of that call to one of these functions; the arrival of the specified time will not cause anything special to happen.

The `list-timers` command lists all the currently active timers. There’s only one command available in the buffer displayed: `c` (`timer-list-cancel`) that will cancel the timer on the line under point.

Next: [Idle Timers](Idle-Timers.html), Previous: [Time Calculations](Time-Calculations.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
