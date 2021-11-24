

#### 23.4.7 Window Header Lines

A window can have a *header line* at the top, just as it can have a mode line at the bottom. The header line feature works just like the mode line feature, except that it’s controlled by `header-line-format`:

### Variable: **header-line-format**

This variable, local in every buffer, specifies how to display the header line, for windows displaying the buffer. The format of the value is the same as for `mode-line-format` (see [Mode Line Data](Mode-Line-Data.html)). It is normally `nil`, so that ordinary buffers have no header line.

### Function: **window-header-line-height** *\&optional window*

This function returns the height in pixels of `window`’s header line. `window` must be a live window, and defaults to the selected window.

A window that is just one line tall never displays a header line. A window that is two lines tall cannot display both a mode line and a header line at once; if it has a mode line, then it does not display a header line.
