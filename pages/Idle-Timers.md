

Next: [Terminal Input](Terminal-Input.html), Previous: [Timers](Timers.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.12 Idle Timers

Here is how to set up a timer that runs when Emacs is idle for a certain length of time. Aside from how to set them up, idle timers work just like ordinary timers.

*   Command: **run-with-idle-timer** *secs repeat function \&rest args*

    Set up a timer which runs the next time Emacs is idle for `secs` seconds. The value of `secs` may be a number or a value of the type returned by `current-idle-time`.

    If `repeat` is `nil`, the timer runs just once, the first time Emacs remains idle for a long enough time. More often `repeat` is non-`nil`, which means to run the timer *each time* Emacs remains idle for `secs` seconds.

    The function `run-with-idle-timer` returns a timer value which you can use in calling `cancel-timer` (see [Timers](Timers.html)).

Emacs becomes *idle* when it starts waiting for user input, and it remains idle until the user provides some input. If a timer is set for five seconds of idleness, it runs approximately five seconds after Emacs first becomes idle. Even if `repeat` is non-`nil`, this timer will not run again as long as Emacs remains idle, because the duration of idleness will continue to increase and will not go down to five seconds again.

Emacs can do various things while idle: garbage collect, autosave or handle data from a subprocess. But these interludes during idleness do not interfere with idle timers, because they do not reset the clock of idleness to zero. An idle timer set for 600 seconds will run when ten minutes have elapsed since the last user command was finished, even if subprocess output has been accepted thousands of times within those ten minutes, and even if there have been garbage collections and autosaves.

When the user supplies input, Emacs becomes non-idle while executing the input. Then it becomes idle again, and all the idle timers that are set up to repeat will subsequently run another time, one by one.

Do not write an idle timer function containing a loop which does a certain amount of processing each time around, and exits when `(input-pending-p)` is non-`nil`. This approach seems very natural but has two problems:

*   It blocks out all process output (since Emacs accepts process output only while waiting).
*   It blocks out any idle timers that ought to run during that time.

Similarly, do not write an idle timer function that sets up another idle timer (including the same idle timer) with `secs` argument less than or equal to the current idleness time. Such a timer will run almost immediately, and continue running again and again, instead of waiting for the next time Emacs becomes idle. The correct approach is to reschedule with an appropriate increment of the current value of the idleness time, as described below.

*   Function: **current-idle-time**

    If Emacs is idle, this function returns the length of time Emacs has been idle, using the same format as `current-time` (see [Time of Day](Time-of-Day.html)).

    When Emacs is not idle, `current-idle-time` returns `nil`. This is a convenient way to test whether Emacs is idle.

The main use of `current-idle-time` is when an idle timer function wants to “take a break” for a while. It can set up another idle timer to call the same function again, after a few seconds more idleness. Here’s an example:

```lisp
(defvar my-resume-timer nil
  "Timer for `my-timer-function' to reschedule itself, or nil.")

(defun my-timer-function ()
  ;; If the user types a command while my-resume-timer
  ;; is active, the next time this function is called from
  ;; its main idle timer, deactivate my-resume-timer.
  (when my-resume-timer
    (cancel-timer my-resume-timer))
  ...do the work for a while...
  (when taking-a-break
    (setq my-resume-timer
          (run-with-idle-timer
            ;; Compute an idle time break-length
            ;; more than the current value.
            (time-add (current-idle-time) break-length)
            nil
            'my-timer-function))))
```

Next: [Terminal Input](Terminal-Input.html), Previous: [Timers](Timers.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
