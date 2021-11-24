

### 29.11 Visibility of Frames

A frame on a graphical display may be *visible*, *invisible*, or *iconified*. If it is visible, its contents are displayed in the usual manner. If it is iconified, its contents are not displayed, but there is a little icon somewhere to bring the frame back into view (some window managers refer to this state as *minimized* rather than *iconified*, but from Emacs’ point of view they are the same thing). If a frame is invisible, it is not displayed at all.

The concept of visibility is strongly related to that of (un-)mapped frames. A frame (or, more precisely, its window-system window) is and becomes *mapped* when it is displayed for the first time and whenever it changes its state of visibility from `iconified` or `invisible` to `visible`. Conversely, a frame is and becomes *unmapped* whenever it changes its status from `visible` to `iconified` or `invisible`.

Visibility is meaningless on text terminals, since only the selected frame is actually displayed in any case.

### Function: **frame-visible-p** *frame*

This function returns the visibility status of frame `frame`. The value is `t` if `frame` is visible, `nil` if it is invisible, and `icon` if it is iconified.

On a text terminal, all frames are considered visible for the purposes of this function, even though only one frame is displayed. See [Raising and Lowering](Raising-and-Lowering.html).

### Command: **iconify-frame** *\&optional frame*

This function iconifies frame `frame`. If you omit `frame`, it iconifies the selected frame. This usually makes all child frames of `frame` (and their descendants) invisible (see [Child Frames](Child-Frames.html)).

### Command: **make-frame-visible** *\&optional frame*

This function makes frame `frame` visible. If you omit `frame`, it makes the selected frame visible. This does not raise the frame, but you can do that with `raise-frame` if you wish (see [Raising and Lowering](Raising-and-Lowering.html)).

Making a frame visible usually makes all its child frames (and their descendants) visible as well (see [Child Frames](Child-Frames.html)).

### Command: **make-frame-invisible** *\&optional frame force*

This function makes frame `frame` invisible. If you omit `frame`, it makes the selected frame invisible. Usually, this makes all child frames of `frame` (and their descendants) invisible too (see [Child Frames](Child-Frames.html)).

Unless `force` is non-`nil`, this function refuses to make `frame` invisible if all other frames are invisible.

The visibility status of a frame is also available as a frame parameter. You can read or change it as such. See [Management Parameters](Management-Parameters.html). The user can also iconify and deiconify frames with the window manager. This happens below the level at which Emacs can exert any control, but Emacs does provide events that you can use to keep track of such changes. See [Misc Events](Misc-Events.html).

### Function: **x-double-buffered-p** *\&optional frame*

This function returns non-`nil` if `frame` is currently being rendered with double buffering. `frame` defaults to the selected frame.
