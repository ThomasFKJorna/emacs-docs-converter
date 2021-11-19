

Next: [Screen Lines](Screen-Lines.html), Previous: [Buffer End Motion](Buffer-End-Motion.html), Up: [Motion](Motion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 30.2.4 Motion by Text Lines

Text lines are portions of the buffer delimited by newline characters, which are regarded as part of the previous line. The first text line begins at the beginning of the buffer, and the last text line ends at the end of the buffer whether or not the last character is a newline. The division of the buffer into text lines is not affected by the width of the window, by line continuation in display, or by how tabs and control characters are displayed.

*   Command: **beginning-of-line** *\&optional count*

    This function moves point to the beginning of the current line. With an argument `count` not `nil` or 1, it moves forward `count`-1 lines and then to the beginning of the line.

    This function does not move point across a field boundary (see [Fields](Fields.html)) unless doing so would move beyond there to a different line; therefore, if `count` is `nil` or 1, and point starts at a field boundary, point does not move. To ignore field boundaries, either bind `inhibit-field-text-motion` to `t`, or use the `forward-line` function instead. For instance, `(forward-line 0)` does the same thing as `(beginning-of-line)`, except that it ignores field boundaries.

    If this function reaches the end of the buffer (or of the accessible portion, if narrowing is in effect), it positions point there. No error is signaled.

<!---->

*   Function: **line-beginning-position** *\&optional count*

    Return the position that `(beginning-of-line count)` would move to.

<!---->

*   Command: **end-of-line** *\&optional count*

    This function moves point to the end of the current line. With an argument `count` not `nil` or 1, it moves forward `count`-1 lines and then to the end of the line.

    This function does not move point across a field boundary (see [Fields](Fields.html)) unless doing so would move beyond there to a different line; therefore, if `count` is `nil` or 1, and point starts at a field boundary, point does not move. To ignore field boundaries, bind `inhibit-field-text-motion` to `t`.

    If this function reaches the end of the buffer (or of the accessible portion, if narrowing is in effect), it positions point there. No error is signaled.

<!---->

*   Function: **line-end-position** *\&optional count*

    Return the position that `(end-of-line count)` would move to.

<!---->

*   Command: **forward-line** *\&optional count*

    This function moves point forward `count` lines, to the beginning of the line following that. If `count` is negative, it moves point -`count` lines backward, to the beginning of a line preceding that. If `count` is zero, it moves point to the beginning of the current line. If `count` is `nil`, that means 1.

    If `forward-line` encounters the beginning or end of the buffer (or of the accessible portion) before finding that many lines, it sets point there. No error is signaled.

    `forward-line` returns the difference between `count` and the number of lines actually moved. If you attempt to move down five lines from the beginning of a buffer that has only three lines, point stops at the end of the last line, and the value will be 2. As an explicit exception, if the last accessible line is non-empty, but has no newline (e.g., if the buffer ends without a newline), the function sets point to the end of that line, and the value returned by the function counts that line as one line successfully moved.

    In an interactive call, `count` is the numeric prefix argument.

<!---->

*   Function: **count-lines** *start end*

    This function returns the number of lines between the positions `start` and `end` in the current buffer. If `start` and `end` are equal, then it returns 0. Otherwise it returns at least 1, even if `start` and `end` are on the same line. This is because the text between them, considered in isolation, must contain at least one line unless it is empty.

<!---->

*   Command: **count-words** *start end*

    This function returns the number of words between the positions `start` and `end` in the current buffer.

    This function can also be called interactively. In that case, it prints a message reporting the number of lines, words, and characters in the buffer, or in the region if the region is active.

<!---->

*   Function: **line-number-at-pos** *\&optional pos absolute*

    This function returns the line number in the current buffer corresponding to the buffer position `pos`. If `pos` is `nil` or omitted, the current buffer position is used. If `absolute` is `nil`, the default, counting starts at `(point-min)`, so the value refers to the contents of the accessible portion of the (potentially narrowed) buffer. If `absolute` is non-`nil`, ignore any narrowing and return the absolute line number.

Also see the functions `bolp` and `eolp` in [Near Point](Near-Point.html). These functions do not move point, but test whether it is already at the beginning or end of a line.

Next: [Screen Lines](Screen-Lines.html), Previous: [Buffer End Motion](Buffer-End-Motion.html), Up: [Motion](Motion.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
