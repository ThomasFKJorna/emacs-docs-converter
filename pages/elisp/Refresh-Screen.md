

### 39.1 Refreshing the Screen

The function `redraw-frame` clears and redisplays the entire contents of a given frame (see [Frames](Frames.html)). This is useful if the screen is corrupted.

### Function: **redraw-frame** *\&optional frame*

This function clears and redisplays frame `frame`. If `frame` is omitted or `nil`, it redraws the selected frame.

Even more powerful is `redraw-display`:

### Command: **redraw-display**

This function clears and redisplays all visible frames.

In Emacs, processing user input takes priority over redisplay. If you call these functions when input is available, they don’t redisplay immediately, but the requested redisplay does happen eventually—after all the input has been processed.

On text terminals, suspending and resuming Emacs normally also refreshes the screen. Some terminal emulators record separate contents for display-oriented programs such as Emacs and for ordinary sequential display. If you are using such a terminal, you might want to inhibit the redisplay on resumption.

### User Option: **no-redraw-on-reenter**

This variable controls whether Emacs redraws the entire screen after it has been suspended and resumed. Non-`nil` means there is no need to redraw, `nil` means redrawing is needed. The default is `nil`.
