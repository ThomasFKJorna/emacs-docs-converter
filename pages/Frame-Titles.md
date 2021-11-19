

Next: [Deleting Frames](Deleting-Frames.html), Previous: [Terminal Parameters](Terminal-Parameters.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.6 Frame Titles

Every frame has a `name` parameter; this serves as the default for the frame title which window systems typically display at the top of the frame. You can specify a name explicitly by setting the `name` frame property.

Normally you don’t specify the name explicitly, and Emacs computes the frame name automatically based on a template stored in the variable `frame-title-format`. Emacs recomputes the name each time the frame is redisplayed.

*   Variable: **frame-title-format**

    This variable specifies how to compute a name for a frame when you have not explicitly specified one. The variable’s value is actually a mode line construct, just like `mode-line-format`, except that the ‘`%c`’, ‘`%C`’, and ‘`%l`’ constructs are ignored. See [Mode Line Data](Mode-Line-Data.html).

<!---->

*   Variable: **icon-title-format**

    This variable specifies how to compute the name for an iconified frame, when you have not explicitly specified the frame title. This title appears in the icon itself.

<!---->

*   Variable: **multiple-frames**

    This variable is set automatically by Emacs. Its value is `t` when there are two or more frames (not counting minibuffer-only frames or invisible frames). The default value of `frame-title-format` uses `multiple-frames` so as to put the buffer name in the frame title only when there is more than one frame.

    The value of this variable is not guaranteed to be accurate except while processing `frame-title-format` or `icon-title-format`.
