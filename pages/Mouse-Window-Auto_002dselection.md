

Next: [Window Configurations](Window-Configurations.html), Previous: [Coordinates and Windows](Coordinates-and-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.25 Mouse Window Auto-selection

The following option allows to automatically select the window under the mouse pointer. This accomplishes a policy similar to that of window managers that give focus to a frame (and thus trigger its subsequent selection) whenever the mouse pointer enters its window-system window (see [Input Focus](Input-Focus.html)).

*   User Option: **mouse-autoselect-window**

    If this variable is non-`nil`, Emacs will try to automatically select the window under the mouse pointer. The following values are meaningful:

    *   A positive number

        This specifies a delay in seconds after which auto-selection triggers. The window under the mouse pointer is selected after the mouse has remained in it for the entire duration of the delay.

    *   A negative number

        A negative number has a similar effect as a positive number, but selects the window under the mouse pointer only after the mouse pointer has remained in it for the entire duration of the absolute value of that number and in addition has stopped moving.

    *   Other value

        Any other non-`nil` value means to select a window instantaneously as soon as the mouse pointer enters it.

    In either case, the mouse pointer must enter the text area of a window in order to trigger its selection. Dragging the scroll bar slider or the mode line of a window conceptually should not cause its auto-selection.

    Mouse auto-selection selects the minibuffer window only if it is active, and never deselects the active minibuffer window.

Mouse auto-selection can be used to emulate a focus follows mouse policy for child frames (see [Child Frames](Child-Frames.html)) which usually are not tracked by the window manager. This requires to set the value of `focus-follows-mouse` (see [Input Focus](Input-Focus.html)) to a non-`nil` value. If the value of `focus-follows-mouse` is `auto-raise`, entering a child frame with the mouse will raise it automatically above all other child frames of that frame’s parent frame.

Next: [Window Configurations](Window-Configurations.html), Previous: [Coordinates and Windows](Coordinates-and-Windows.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
