

Next: [Window Systems](Window-Systems.html), Previous: [Character Display](Character-Display.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.23 Beeping

This section describes how to make Emacs ring the bell (or blink the screen) to attract the user’s attention. Be conservative about how often you do this; frequent bells can become irritating. Also be careful not to use just beeping when signaling an error is more appropriate (see [Errors](Errors.html)).

*   Function: **ding** *\&optional do-not-terminate*

    This function beeps, or flashes the screen (see `visible-bell` below). It also terminates any keyboard macro currently executing unless `do-not-terminate` is non-`nil`.

<!---->

*   Function: **beep** *\&optional do-not-terminate*

    This is a synonym for `ding`.

<!---->

*   User Option: **visible-bell**

    This variable determines whether Emacs should flash the screen to represent a bell. Non-`nil` means yes, `nil` means no. This is effective on graphical displays, and on text terminals provided the terminal’s Termcap entry defines the visible bell capability (‘`vb`’).

<!---->

*   User Option: **ring-bell-function**

    If this is non-`nil`, it specifies how Emacs should ring the bell. Its value should be a function of no arguments. If this is non-`nil`, it takes precedence over the `visible-bell` variable.
