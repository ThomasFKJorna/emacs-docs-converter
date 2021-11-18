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

Next: [Logging Messages](Logging-Messages.html), Previous: [Displaying Messages](Displaying-Messages.html), Up: [The Echo Area](The-Echo-Area.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.4.2 Reporting Operation Progress

When an operation can take a while to finish, you should inform the user about the progress it makes. This way the user can estimate remaining time and clearly see that Emacs is busy working, not hung. A convenient way to do this is to use a *progress reporter*.

Here is a working example that does nothing useful:

    (let ((progress-reporter
           (make-progress-reporter "Collecting mana for Emacs..."
                                   0  500)))
      (dotimes (k 500)
        (sit-for 0.01)
        (progress-reporter-update progress-reporter k))
      (progress-reporter-done progress-reporter))

*   Function: **make-progress-reporter** *message \&optional min-value max-value current-value min-change min-time*

    This function creates and returns a progress reporter object, which you will use as an argument for the other functions listed below. The idea is to precompute as much data as possible to make progress reporting very fast.

    When this progress reporter is subsequently used, it will display `message` in the echo area, followed by progress percentage. `message` is treated as a simple string. If you need it to depend on a filename, for instance, use `format-message` before calling this function.

    The arguments `min-value` and `max-value` should be numbers standing for the starting and final states of the operation. For instance, an operation that scans a buffer should set these to the results of `point-min` and `point-max` correspondingly. `max-value` should be greater than `min-value`.

    Alternatively, you can set `min-value` and `max-value` to `nil`. In that case, the progress reporter does not report process percentages; it instead displays a “spinner” that rotates a notch each time you update the progress reporter.

    If `min-value` and `max-value` are numbers, you can give the argument `current-value` a numerical value specifying the initial progress; if omitted, this defaults to `min-value`.

    The remaining arguments control the rate of echo area updates. The progress reporter will wait for at least `min-change` more percents of the operation to be completed before printing next message; the default is one percent. `min-time` specifies the minimum time in seconds to pass between successive prints; the default is 0.2 seconds. (On some operating systems, the progress reporter may handle fractions of seconds with varying precision).

    This function calls `progress-reporter-update`, so the first message is printed immediately.

<!---->

*   Function: **progress-reporter-update** *reporter \&optional value suffix*

    This function does the main work of reporting progress of your operation. It displays the message of `reporter`, followed by progress percentage determined by `value`. If percentage is zero, or close enough according to the `min-change` and `min-time` arguments, then it is omitted from the output.

    `reporter` must be the result of a call to `make-progress-reporter`. `value` specifies the current state of your operation and must be between `min-value` and `max-value` (inclusive) as passed to `make-progress-reporter`. For instance, if you scan a buffer, then `value` should be the result of a call to `point`.

    Optional argument `suffix` is a string to be displayed after `reporter`’s main message and progress text. If `reporter` is a non-numerical reporter, then `value` should be `nil`, or a string to use instead of `suffix`.

    This function respects `min-change` and `min-time` as passed to `make-progress-reporter` and so does not output new messages on every invocation. It is thus very fast and normally you should not try to reduce the number of calls to it: resulting overhead will most likely negate your effort.

<!---->

*   Function: **progress-reporter-force-update** *reporter \&optional value new-message suffix*

    This function is similar to `progress-reporter-update` except that it prints a message in the echo area unconditionally.

    `reporter`, `value`, and `suffix` have the same meaning as for `progress-reporter-update`. Optional `new-message` allows you to change the message of the `reporter`. Since this function always updates the echo area, such a change will be immediately presented to the user.

<!---->

*   Function: **progress-reporter-done** *reporter*

    This function should be called when the operation is finished. It prints the message of `reporter` followed by word ‘`done`’ in the echo area.

    You should always call this function and not hope for `progress-reporter-update` to print ‘`100%`’. Firstly, it may never print it, there are many good reasons for this not to happen. Secondly, ‘`done`’ is more explicit.

<!---->

*   Macro: **dotimes-with-progress-reporter** *(var count \[result]) reporter-or-message body…*

    This is a convenience macro that works the same way as `dotimes` does, but also reports loop progress using the functions described above. It allows you to save some typing. The argument `reporter-or-message` can be either a string or a progress reporter object.

    You can rewrite the example in the beginning of this subsection using this macro as follows:

        (dotimes-with-progress-reporter
            (k 500)
            "Collecting some mana for Emacs..."
          (sit-for 0.01))

    Using a reporter object as the `reporter-or-message` argument is useful if you want to specify the optional arguments in `make-progress-reporter`. For instance, you can write the previous example as follows:

        (dotimes-with-progress-reporter
            (k 500)
            (make-progress-reporter "Collecting some mana for Emacs..." 0 500 0 1 1.5)
          (sit-for 0.01))

<!---->

*   Macro: **dolist-with-progress-reporter** *(var count \[result]) reporter-or-message body…*

    This is another convenience macro that works the same way as `dolist` does, but also reports loop progress using the functions described above. As in `dotimes-with-progress-reporter`, `reporter-or-message` can be a progress reporter or a string. You can rewrite the previous example with this macro as follows:

        (dolist-with-progress-reporter
            (k (number-sequence 0 500))
            "Collecting some mana for Emacs..."
          (sit-for 0.01))

Next: [Logging Messages](Logging-Messages.html), Previous: [Displaying Messages](Displaying-Messages.html), Up: [The Echo Area](The-Echo-Area.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
