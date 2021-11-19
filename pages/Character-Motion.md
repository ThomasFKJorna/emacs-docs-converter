

Next: [Word Motion](Word-Motion.html), Up: [Motion](Motion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 30.2.1 Motion by Characters

These functions move point based on a count of characters. `goto-char` is the fundamental primitive; the other functions use that.

*   Command: **goto-char** *position*

    This function sets point in the current buffer to the value `position`.

    If narrowing is in effect, `position` still counts from the beginning of the buffer, but point cannot go outside the accessible portion. If `position` is out of range, `goto-char` moves point to the beginning or the end of the accessible portion.

    When this function is called interactively, `position` is the numeric prefix argument, if provided; otherwise it is read from the minibuffer.

    `goto-char` returns `position`.

<!---->

*   Command: **forward-char** *\&optional count*

    This function moves point `count` characters forward, towards the end of the buffer (or backward, towards the beginning of the buffer, if `count` is negative). If `count` is `nil`, the default is 1.

    If this attempts to move past the beginning or end of the buffer (or the limits of the accessible portion, when narrowing is in effect), it signals an error with error symbol `beginning-of-buffer` or `end-of-buffer`.

    In an interactive call, `count` is the numeric prefix argument.

<!---->

*   Command: **backward-char** *\&optional count*

    This is just like `forward-char` except that it moves in the opposite direction.
