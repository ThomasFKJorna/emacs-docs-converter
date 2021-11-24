

### 29.12 Raising, Lowering and Restacking Frames

Most window systems use a desktop metaphor. Part of this metaphor is the idea that system-level windows (representing, e.g., Emacs frames) are stacked in a notional third dimension perpendicular to the screen surface. The order induced by stacking is total and usually referred to as stacking (or Z-) order. Where the areas of two windows overlap, the one higher up in that order will (partially) cover the one underneath.

You can *raise* a frame to the top of that order or *lower* a frame to its bottom by using the functions `raise-frame` and `lower-frame`. You can *restack* a frame directly above or below another frame using the function `frame-restack`.

Note that all functions described below will respect the adherence of frames (and all other window-system windows) to their respective z-group (see [Position Parameters](Position-Parameters.html)). For example, you usually cannot lower a frame below that of the desktop window and you cannot raise a frame whose `z-group` parameter is `nil` above the window-system’s taskbar or tooltip window.

### Command: **raise-frame** *\&optional frame*

This function raises frame `frame` (default, the selected frame) above all other frames belonging to the same or a lower z-group as `frame`. If `frame` is invisible or iconified, this makes it visible. If `frame` is a child frame (see [Child Frames](Child-Frames.html)), this raises `frame` above all other child frames of its parent.

### Command: **lower-frame** *\&optional frame*

This function lowers frame `frame` (default, the selected frame) below all other frames belonging to the same or a higher z-group as `frame`. If `frame` is a child frame (see [Child Frames](Child-Frames.html)), this lowers `frame` below all other child frames of its parent.

### Function: **frame-restack** *frame1 frame2 \&optional above*

This function restacks `frame1` below `frame2`. This implies that if both frames are visible and their display areas overlap, `frame2` will (partially) obscure `frame1`. If the optional third argument `above` is non-`nil`, this function restacks `frame1` above `frame2`. This means that if both frames are visible and their display areas overlap, `frame1` will (partially) obscure `frame2`.

Technically, this function may be thought of as an atomic action performed in two steps: The first step removes `frame1`’s window-system window from the display. The second step reinserts `frame1`’s window into the display below (above if `above` is true) that of `frame2`. Hence the position of `frame2` in its display’s Z (stacking) order relative to all other frames excluding `frame1` remains unaltered.

Some window managers may refuse to restack windows.

Note that the effect of restacking will only hold as long as neither of the involved frames is iconified or made invisible. You can use the `z-group` (see [Position Parameters](Position-Parameters.html)) frame parameter to add a frame to a group of frames permanently shown above or below other frames. As long as a frame belongs to one of these groups, restacking it will only affect its relative stacking position within that group. The effect of restacking frames belonging to different z-groups is undefined. You can list frames in their current stacking order with the function `frame-list-z-order` (see [Finding All Frames](Finding-All-Frames.html)).

### User Option: **minibuffer-auto-raise**

If this is non-`nil`, activation of the minibuffer raises the frame that the minibuffer window is in.

On window systems, you can also enable auto-raising (on frame selection) or auto-lowering (on frame deselection) using frame parameters. See [Management Parameters](Management-Parameters.html).

The concept of raising and lowering frames also applies to text terminal frames. On each text terminal, only the top frame is displayed at any one time.

### Function: **tty-top-frame** *\&optional terminal*

This function returns the top frame on `terminal`. `terminal` should be a terminal object, a frame (meaning that frame’s terminal), or `nil` (meaning the selected frame’s terminal). If it does not refer to a text terminal, the return value is `nil`.
