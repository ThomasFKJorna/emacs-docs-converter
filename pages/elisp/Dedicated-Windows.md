

### 28.15 Dedicated Windows

Functions for displaying a buffer can be told to not use specific windows by marking these windows as *dedicated* to their buffers. `display-buffer` (see [Choosing Window](Choosing-Window.html)) never uses a dedicated window for displaying another buffer in it. `get-lru-window` and `get-largest-window` (see [Cyclic Window Ordering](Cyclic-Window-Ordering.html)) do not consider dedicated windows as candidates when their `dedicated` argument is non-`nil`. The behavior of `set-window-buffer` (see [Buffers and Windows](Buffers-and-Windows.html)) with respect to dedicated windows is slightly different, see below.

Functions supposed to remove a buffer from a window or a window from a frame can behave specially when a window they operate on is dedicated. We will distinguish three basic cases, namely where (1) the window is not the only window on its frame, (2) the window is the only window on its frame but there are other frames on the same terminal left, and (3) the window is the only window on the only frame on the same terminal.

In particular, `delete-windows-on` (see [Deleting Windows](Deleting-Windows.html)) handles case (2) by deleting the associated frame and case (3) by showing another buffer in that frame’s only window. The function `replace-buffer-in-windows` (see [Buffers and Windows](Buffers-and-Windows.html)) which is called when a buffer gets killed, deletes the window in case (1) and behaves like `delete-windows-on` otherwise.

When `bury-buffer` (see [Buffer List](Buffer-List.html)) operates on the selected window (which shows the buffer that shall be buried), it handles case (2) by calling `frame-auto-hide-function` (see [Quitting Windows](Quitting-Windows.html)) to deal with the selected frame. The other two cases are handled as with `replace-buffer-in-windows`.

### Function: **window-dedicated-p** *\&optional window*

This function returns non-`nil` if `window` is dedicated to its buffer and `nil` otherwise. More precisely, the return value is the value assigned by the last call of `set-window-dedicated-p` for `window`, or `nil` if that function was never called with `window` as its argument. The default for `window` is the selected window.

### Function: **set-window-dedicated-p** *window flag*

This function marks `window` as dedicated to its buffer if `flag` is non-`nil`, and non-dedicated otherwise.

As a special case, if `flag` is `t`, `window` becomes *strongly* dedicated to its buffer. `set-window-buffer` signals an error when the window it acts upon is strongly dedicated to its buffer and does not already display the buffer it is asked to display. Other functions do not treat `t` differently from any non-`nil` value.

You can also tell `display-buffer` to mark a window it creates as dedicated to its buffer by providing a suitable `dedicated` action alist entry (see [Buffer Display Action Alists](Buffer-Display-Action-Alists.html)).
