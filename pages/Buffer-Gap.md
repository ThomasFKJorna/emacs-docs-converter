

Previous: [Swapping Text](Swapping-Text.html), Up: [Buffers](Buffers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 27.13 The Buffer Gap

Emacs buffers are implemented using an invisible *gap* to make insertion and deletion faster. Insertion works by filling in part of the gap, and deletion adds to the gap. Of course, this means that the gap must first be moved to the locus of the insertion or deletion. Emacs moves the gap only when you try to insert or delete. This is why your first editing command in one part of a large buffer, after previously editing in another far-away part, sometimes involves a noticeable delay.

This mechanism works invisibly, and Lisp code should never be affected by the gap’s current location, but these functions are available for getting information about the gap status.

*   Function: **gap-position**

    This function returns the current gap position in the current buffer.

<!---->

*   Function: **gap-size**

    This function returns the current gap size of the current buffer.
