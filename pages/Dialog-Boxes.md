

Next: [Pointer Shape](Pointer-Shape.html), Previous: [Pop-Up Menus](Pop_002dUp-Menus.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.18 Dialog Boxes

A dialog box is a variant of a pop-up menu—it looks a little different, it always appears in the center of a frame, and it has just one level and one or more buttons. The main use of dialog boxes is for asking questions that the user can answer with “yes”, “no”, and a few other alternatives. With a single button, they can also force the user to acknowledge important information. The functions `y-or-n-p` and `yes-or-no-p` use dialog boxes instead of the keyboard, when called from commands invoked by mouse clicks.

*   Function: **x-popup-dialog** *position contents \&optional header*

    This function displays a pop-up dialog box and returns an indication of what selection the user makes. The argument `contents` specifies the alternatives to offer; it has this format:

    ```lisp
    (title (string . value)…)
    ```

    which looks like the list that specifies a single pane for `x-popup-menu`.

    The return value is `value` from the chosen alternative.

    As for `x-popup-menu`, an element of the list may be just a string instead of a cons cell `(string . value)`. That makes a box that cannot be selected.

    If `nil` appears in the list, it separates the left-hand items from the right-hand items; items that precede the `nil` appear on the left, and items that follow the `nil` appear on the right. If you don’t include a `nil` in the list, then approximately half the items appear on each side.

    Dialog boxes always appear in the center of a frame; the argument `position` specifies which frame. The possible values are as in `x-popup-menu`, but the precise coordinates or the individual window don’t matter; only the frame matters.

    If `header` is non-`nil`, the frame title for the box is ‘`Information`’, otherwise it is ‘`Question`’. The former is used for `message-box` (see [message-box](Displaying-Messages.html#message_002dbox)). (On text terminals, the box title is not displayed.)

    In some configurations, Emacs cannot display a real dialog box; so instead it displays the same items in a pop-up menu in the center of the frame.

    If the user gets rid of the dialog box without making a valid choice, for instance using the window manager, then this produces a quit and `x-popup-dialog` does not return.

Next: [Pointer Shape](Pointer-Shape.html), Previous: [Pop-Up Menus](Pop_002dUp-Menus.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
