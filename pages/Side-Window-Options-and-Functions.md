

Next: [Frame Layouts with Side Windows](Frame-Layouts-with-Side-Windows.html), Previous: [Displaying Buffers in Side Windows](Displaying-Buffers-in-Side-Windows.html), Up: [Side Windows](Side-Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 28.17.2 Side Window Options and Functions

The following options provide additional control over the placement of side windows.

*   User Option: **window-sides-vertical**

    If non-`nil`, the side windows on the left and right of a frame occupy the frame’s full height. Otherwise, the side windows on the top and bottom of the frame occupy the frame’s full width.

<!---->

*   User Option: **window-sides-slots**

    This option specifies the maximum number of side windows on each side of a frame. The value is a list of four elements specifying the number of side window slots on (in this order) the left, top, right and bottom of each frame. If an element is a number, it means to display at most that many windows on the corresponding side. If an element is `nil`, it means there’s no bound on the number of slots on that side.

    If any of the specified values is zero, no window can be created on the corresponding side. `display-buffer-in-side-window` will not signal an error in that case, but will return `nil`. If a specified value just forbids the creation of an additional side window, the most suitable window on that side is reused and may have its `window-slot` parameter changed accordingly.

<!---->

*   User Option: **window-sides-reversed**

    This option specifies whether top/bottom side windows should appear in reverse order. When this is `nil`, side windows on the top and bottom of a frame are always drawn from left to right with increasing slot values. When this is `t`, the drawing order is reversed and side windows on the top and bottom of a frame are drawn from right to left with increasing slot values.

    When this is `bidi`, the drawing order is reversed if and only if the value of `bidi-paragraph-direction` (see [Bidirectional Display](Bidirectional-Display.html)) is `right-to-left` in the buffer displayed in the window most recently selected within the main window area of this frame. Sometimes that window may be hard to find, so heuristics are used to avoid that the drawing order changes inadvertently when another window gets selected.

    The layout of side windows on the left or right of a frame is not affected by the value of this variable.

When a frame has side windows, the following function returns the main window of that frame.

*   Function: **window-main-window** *\&optional frame*

    This function returns the main window of the specified `frame`. The optional argument `frame` must be a live frame and defaults to the selected one.

    If `frame` has no side windows, it returns `frame`’s root window. Otherwise, it returns either an internal non-side window such that all other non-side windows on `frame` descend from it, or the single live non-side window of `frame`. Note that the main window of a frame cannot be deleted via `delete-window`.

The following command is handy to toggle the appearance of all side windows on a specified frame.

*   Command: **window-toggle-side-windows** *\&optional frame*

    This command toggles side windows on the specified `frame`. The optional argument `frame` must be a live frame and defaults to the selected one.

    If `frame` has at least one side window, this command saves the state of `frame`’s root window in the `frame`’s `window-state` frame parameter and deletes all side windows on `frame` afterwards.

    If `frame` has no side windows, but does have a `window-state` parameter, this command uses that parameter’s value to restore the side windows on `frame` leaving `frame`’s main window alone.

    An error is signaled if `frame` has no side windows and no saved state is found for it.

Next: [Frame Layouts with Side Windows](Frame-Layouts-with-Side-Windows.html), Previous: [Displaying Buffers in Side Windows](Displaying-Buffers-in-Side-Windows.html), Up: [Side Windows](Side-Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
