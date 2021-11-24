

#### 29.4.3.7 Mouse Dragging Parameters

The parameters described below provide support for resizing a frame by dragging its internal borders with the mouse. They also allow moving a frame with the mouse by dragging the header line of its topmost or the mode line of its bottommost window.

These parameters are mostly useful for child frames (see [Child Frames](Child-Frames.html)) that come without window manager decorations. If necessary, they can be used for undecorated top-level frames as well.

`drag-internal-border`

If non-`nil`, the frame can be resized by dragging its internal borders, if present, with the mouse.

`drag-with-header-line`

If non-`nil`, the frame can be moved with the mouse by dragging the header line of its topmost window.

`drag-with-mode-line`

If non-`nil`, the frame can be moved with the mouse by dragging the mode line of its bottommost window. Note that such a frame is not allowed to have its own minibuffer window.

`snap-width`

A frame that is moved with the mouse will “snap” at the border(s) of the display or its parent frame whenever it is dragged as near to such an edge as the number of pixels specified by this parameter.

`top-visible`

If this parameter is a number, the top edge of the frame never appears above the top edge of its display or parent frame. Moreover, as many pixels of the frame as specified by that number will remain visible when the frame is moved against any of the remaining edges of its display or parent frame. Setting this parameter is useful to guard against dragging a child frame with a non-`nil` `drag-with-header-line` parameter completely out of the area of its parent frame.

`bottom-visible`

If this parameter is a number, the bottom edge of the frame never appears below the bottom edge of its display or parent frame. Moreover, as many pixels of the frame as specified by that number will remain visible when the frame is moved against any of the remaining edges of its display or parent frame. Setting this parameter is useful to guard against dragging a child frame with a non-`nil` `drag-with-mode-line` parameter completely out of the area of its parent frame.
