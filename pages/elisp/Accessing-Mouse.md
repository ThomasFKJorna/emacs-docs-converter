

#### 21.7.13 Accessing Mouse Events

This section describes convenient functions for accessing the data in a mouse button or motion event. Keyboard event data can be accessed using the same functions, but data elements that aren’t applicable to keyboard events are zero or `nil`.

The following two functions return a mouse position list (see [Click Events](Click-Events.html)), specifying the position of a mouse event.

### Function: **event-start** *event*

This returns the starting position of `event`.

If `event` is a click or button-down event, this returns the location of the event. If `event` is a drag event, this returns the drag’s starting position.

### Function: **event-end** *event*

This returns the ending position of `event`.

If `event` is a drag event, this returns the position where the user released the mouse button. If `event` is a click or button-down event, the value is actually the starting position, which is the only position such events have.

### Function: **posnp** *object*

This function returns non-`nil` if `object` is a mouse position list, in the format documented in [Click Events](Click-Events.html)); and `nil` otherwise.

These functions take a mouse position list as argument, and return various parts of it:

### Function: **posn-window** *position*

Return the window that `position` is in. If `position` represents a location outside the frame where the event was initiated, return that frame instead.

### Function: **posn-area** *position*

Return the window area recorded in `position`. It returns `nil` when the event occurred in the text area of the window; otherwise, it is a symbol identifying the area in which the event occurred.

### Function: **posn-point** *position*

Return the buffer position in `position`. When the event occurred in the text area of the window, in a marginal area, or on a fringe, this is an integer specifying a buffer position. Otherwise, the value is undefined.

### Function: **posn-x-y** *position*

Return the pixel-based x and y coordinates in `position`, as a cons cell `(x . y)`. These coordinates are relative to the window given by `posn-window`.

This example shows how to convert the window-relative coordinates in the text area of a window into frame-relative coordinates:

```lisp
(defun frame-relative-coordinates (position)
  "Return frame-relative coordinates from POSITION.
POSITION is assumed to lie in a window text area."
  (let* ((x-y (posn-x-y position))
         (window (posn-window position))
         (edges (window-inside-pixel-edges window)))
    (cons (+ (car x-y) (car edges))
          (+ (cdr x-y) (cadr edges)))))
```

### Function: **posn-col-row** *position*

This function returns a cons cell `(col .  row)`, containing the estimated column and row corresponding to buffer position described by `position`. The return value is given in units of the frame’s default character width and default line height (including spacing), as computed from the `x` and `y` values corresponding to `position`. (So, if the actual characters have non-default sizes, the actual row and column may differ from these computed values.)

Note that `row` is counted from the top of the text area. If the window given by `position` possesses a header line (see [Header Lines](Header-Lines.html)) or a tab line, they are *not* included in the `row` count.

### Function: **posn-actual-col-row** *position*

Return the actual row and column in `position`, as a cons cell `(col . row)`. The values are the actual row and column numbers in the window given by `position`. See [Click Events](Click-Events.html), for details. The function returns `nil` if `position` does not include actual position values; in that case `posn-col-row` can be used to get approximate values.

Note that this function doesn’t account for the visual width of characters on display, like the number of visual columns taken by a tab character or an image. If you need the coordinates in canonical character units, use `posn-col-row` instead.

### Function: **posn-string** *position*

Return the string object described by `position`, either `nil` (which means `position` describes buffer text), or a cons cell `(string . string-pos)`.

### Function: **posn-image** *position*

Return the image object in `position`, either `nil` (if there’s no image at `position`), or an image spec `(image …)`.

### Function: **posn-object** *position*

Return the image or string object described by `position`, either `nil` (which means `position` describes buffer text), an image `(image …)`, or a cons cell `(string . string-pos)`.

### Function: **posn-object-x-y** *position*

Return the pixel-based x and y coordinates relative to the upper left corner of the object described by `position`, as a cons cell `(dx . dy)`. If the `position` describes buffer text, return the relative coordinates of the buffer-text character closest to that position.

### Function: **posn-object-width-height** *position*

Return the pixel width and height of the object described by `position`, as a cons cell `(width . height)`. If the `position` describes a buffer position, return the size of the character at that position.

### Function: **posn-timestamp** *position*

Return the timestamp in `position`. This is the time at which the event occurred, in milliseconds.

These functions compute a position list given particular buffer position or screen position. You can access the data in this position list with the functions described above.

### Function: **posn-at-point** *\&optional pos window*

This function returns a position list for position `pos` in `window`. `pos` defaults to point in `window`; `window` defaults to the selected window.

`posn-at-point` returns `nil` if `pos` is not visible in `window`.

### Function: **posn-at-x-y** *x y \&optional frame-or-window whole*

This function returns position information corresponding to pixel coordinates `x` and `y` in a specified frame or window, `frame-or-window`, which defaults to the selected window. The coordinates `x` and `y` are relative to the frame or window used. If `whole` is `nil`, the coordinates are relative to the window text area, otherwise they are relative to the entire window area including scroll bars, margins and fringes.
