

### 28.19 Windows and Point

Each window has its own value of point (see [Point](Point.html)), independent of the value of point in other windows displaying the same buffer. This makes it useful to have multiple windows showing one buffer.

The window point is established when a window is first created; it is initialized from the buffer’s point, or from the window point of another window opened on the buffer if such a window exists.

Selecting a window sets the value of point in its buffer from the window’s value of point. Conversely, deselecting a window sets the window’s value of point from that of the buffer. Thus, when you switch between windows that display a given buffer, the point value for the selected window is in effect in the buffer, while the point values for the other windows are stored in those windows.

As long as the selected window displays the current buffer, the window’s point and the buffer’s point always move together; they remain equal.

Emacs displays the cursor, by default as a rectangular block, in each window at the position of that window’s point. When the user switches to another buffer in a window, Emacs moves that window’s cursor to where point is in that buffer. If the exact position of point is hidden behind some display element, such as a display string or an image, Emacs displays the cursor immediately before or after that display element.

### Function: **window-point** *\&optional window*

This function returns the current position of point in `window`. For a nonselected window, this is the value point would have (in that window’s buffer) if that window were selected. The default for `window` is the selected window.

When `window` is the selected window, the value returned is the value of point in that window’s buffer. Strictly speaking, it would be more correct to return the top-level value of point, outside of any `save-excursion` forms. But that value is hard to find.

### Function: **set-window-point** *window position*

This function positions point in `window` at position `position` in `window`’s buffer. It returns `position`.

If `window` is selected, this simply does `goto-char` in `window`’s buffer.

### Variable: **window-point-insertion-type**

This variable specifies the marker insertion type (see [Marker Insertion Types](Marker-Insertion-Types.html)) of `window-point`. The default is `nil`, so `window-point` will stay behind text inserted there.
