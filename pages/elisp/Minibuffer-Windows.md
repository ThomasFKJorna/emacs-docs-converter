

### 20.11 Minibuffer Windows

These functions access and select minibuffer windows, test whether they are active and control how they get resized.

### Function: **minibuffer-window** *\&optional frame*

This function returns the minibuffer window used for frame `frame`. If `frame` is `nil`, that stands for the selected frame.

Note that the minibuffer window used by a frame need not be part of that frame—a frame that has no minibuffer of its own necessarily uses some other frame’s minibuffer window. The minibuffer window of a minibuffer-less frame can be changed by setting that frame’s `minibuffer` frame parameter (see [Buffer Parameters](Buffer-Parameters.html)).

### Function: **set-minibuffer-window** *window*

This function specifies `window` as the minibuffer window to use. This affects where the minibuffer is displayed if you put text in it without invoking the usual minibuffer commands. It has no effect on the usual minibuffer input functions because they all start by choosing the minibuffer window according to the selected frame.

### Function: **window-minibuffer-p** *\&optional window*

This function returns `t` if `window` is a minibuffer window. `window` defaults to the selected window.

The following function returns the window showing the currently active minibuffer.

### Function: **active-minibuffer-window**

This function returns the window of the currently active minibuffer, or `nil` if there is no active minibuffer.

It is not sufficient to determine whether a given window shows the currently active minibuffer by comparing it with the result of `(minibuffer-window)`, because there can be more than one minibuffer window if there is more than one frame.

### Function: **minibuffer-window-active-p** *window*

This function returns non-`nil` if `window` shows the currently active minibuffer.

The following two options control whether minibuffer windows are resized automatically and how large they can get in the process.

### User Option: **resize-mini-windows**

This option specifies whether minibuffer windows are resized automatically. The default value is `grow-only`, which means that a minibuffer window by default expands automatically to accommodate the text it displays and shrinks back to one line as soon as the minibuffer gets empty. If the value is `t`, Emacs will always try to fit the height of a minibuffer window to the text it displays (with a minimum of one line). If the value is `nil`, a minibuffer window never changes size automatically. In that case the window resizing commands (see [Resizing Windows](Resizing-Windows.html)) can be used to adjust its height.

### User Option: **max-mini-window-height**

This option provides a maximum height for resizing minibuffer windows automatically. A floating-point number specifies the maximum height as a fraction of the frame’s height; an integer specifies the maximum height in units of the frame’s canonical character height (see [Frame Font](Frame-Font.html)). The default value is 0.25.

Note that the values of the above two variables take effect at display time, so let-binding them around code which produces echo-area messages will not work. If you want to prevent resizing of minibuffer windows when displaying long messages, bind the `message-truncate-lines` variable instead (see [Echo Area Customization](Echo-Area-Customization.html)).

The option `resize-mini-windows` does not affect the behavior of minibuffer-only frames (see [Frame Layout](Frame-Layout.html)). The following option allows to automatically resize such frames as well.

### User Option: **resize-mini-frames**

If this is `nil`, minibuffer-only frames are never resized automatically.

If this is a function, that function is called with the minibuffer-only frame to be resized as sole argument. At the time this function is called, the buffer of the minibuffer window of that frame is the buffer whose contents will be shown the next time that window is redisplayed. The function is expected to fit the frame to the buffer in some appropriate way.

Any other non-`nil` value means to resize minibuffer-only frames by calling `fit-mini-frame-to-buffer`, a function that behaves like `fit-frame-to-buffer` (see [Resizing Windows](Resizing-Windows.html)) but does not strip leading or trailing empty lines from the buffer text.
