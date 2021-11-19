

Next: [Pop-Up Menus](Pop_002dUp-Menus.html), Previous: [Mouse Tracking](Mouse-Tracking.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 29.16 Mouse Position

The functions `mouse-position` and `set-mouse-position` give access to the current position of the mouse.

*   Function: **mouse-position**

    This function returns a description of the position of the mouse. The value looks like `(frame x . y)`, where `x` and `y` are integers giving the (possibly rounded) position in multiples of the default character size of `frame` (see [Frame Font](Frame-Font.html)) relative to the native position of `frame` (see [Frame Geometry](Frame-Geometry.html)).

<!---->

*   Variable: **mouse-position-function**

    If non-`nil`, the value of this variable is a function for `mouse-position` to call. `mouse-position` calls this function just before returning, with its normal return value as the sole argument, and it returns whatever this function returns to it.

    This abnormal hook exists for the benefit of packages like `xt-mouse.el` that need to do mouse handling at the Lisp level.

<!---->

*   Function: **set-mouse-position** *frame x y*

    This function *warps the mouse* to position `x`, `y` in frame `frame`. The arguments `x` and `y` are integers, giving the position in multiples of the default character size of `frame` (see [Frame Font](Frame-Font.html)) relative to the native position of `frame` (see [Frame Geometry](Frame-Geometry.html)).

    The resulting mouse position is constrained to the native frame of `frame`. If `frame` is not visible, this function does nothing. The return value is not significant.

<!---->

*   Function: **mouse-pixel-position**

    This function is like `mouse-position` except that it returns coordinates in units of pixels rather than units of characters.

<!---->

*   Function: **set-mouse-pixel-position** *frame x y*

    This function warps the mouse like `set-mouse-position` except that `x` and `y` are in units of pixels rather than units of characters.

    The resulting mouse position is not constrained to the native frame of `frame`. If `frame` is not visible, this function does nothing. The return value is not significant.

On a graphical terminal the following two functions allow the absolute position of the mouse cursor to be retrieved and set.

*   Function: **mouse-absolute-pixel-position**

    This function returns a cons cell (`x` . `y`) of the coordinates of the mouse cursor position in pixels, relative to a position (0, 0) of the selected frame’s display.

<!---->

*   Function: **set-mouse-absolute-pixel-position** *x y*

    This function moves the mouse cursor to the position (`x`, `y`). The coordinates `x` and `y` are interpreted in pixels relative to a position (0, 0) of the selected frame’s display.

The following function can tell whether the mouse cursor is currently visible on a frame:

*   Function: **frame-pointer-visible-p** *\&optional frame*

    This predicate function returns non-`nil` if the mouse pointer displayed on `frame` is visible; otherwise it returns `nil`. `frame` omitted or `nil` means the selected frame. This is useful when `make-pointer-invisible` is set to `t`: it allows you to know if the pointer has been hidden. See [Mouse Avoidance](https://www.gnu.org/software/emacs/manual/html_node/emacs/Mouse-Avoidance.html#Mouse-Avoidance) in The Emacs Manual.

Next: [Pop-Up Menus](Pop_002dUp-Menus.html), Previous: [Mouse Tracking](Mouse-Tracking.html), Up: [Frames](Frames.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
