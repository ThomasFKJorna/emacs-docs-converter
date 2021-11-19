

Next: [Frame Interaction Parameters](Frame-Interaction-Parameters.html), Previous: [Layout Parameters](Layout-Parameters.html), Up: [Window Frame Parameters](Window-Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 29.4.3.5 Buffer Parameters

These frame parameters, meaningful on all kinds of terminals, deal with which buffers have been, or should, be displayed in the frame.

*   `minibuffer`

    Whether this frame has its own minibuffer. The value `t` means yes, `nil` means no, `only` means this frame is just a minibuffer. If the value is a minibuffer window (in some other frame), the frame uses that minibuffer.

    This parameter takes effect when the frame is created. If specified as `nil`, Emacs will try to set it to the minibuffer window of `default-minibuffer-frame` (see [Minibuffers and Frames](Minibuffers-and-Frames.html)). For an existing frame, this parameter can be used exclusively to specify another minibuffer window. It is not allowed to change it from a minibuffer window to `t` and vice-versa, or from `t` to `nil`. If the parameter specifies a minibuffer window already, setting it to `nil` has no effect.

    The special value `child-frame` means to make a minibuffer-only child frame (see [Child Frames](Child-Frames.html)) whose parent becomes the frame created. As if specified as `nil`, Emacs will set this parameter to the minibuffer window of the child frame but will not select the child frame after its creation.

*   `buffer-predicate`

    The buffer-predicate function for this frame. The function `other-buffer` uses this predicate (from the selected frame) to decide which buffers it should consider, if the predicate is not `nil`. It calls the predicate with one argument, a buffer, once for each buffer; if the predicate returns a non-`nil` value, it considers that buffer.

*   `buffer-list`

    A list of buffers that have been selected in this frame, ordered most-recently-selected first.

*   `unsplittable`

    If non-`nil`, this frame’s window is never split automatically.

Next: [Frame Interaction Parameters](Frame-Interaction-Parameters.html), Previous: [Layout Parameters](Layout-Parameters.html), Up: [Window Frame Parameters](Window-Frame-Parameters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
