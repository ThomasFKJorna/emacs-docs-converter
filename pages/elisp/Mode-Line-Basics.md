

#### 23.4.1 Mode Line Basics

The contents of each mode line are specified by the buffer-local variable `mode-line-format` (see [Mode Line Top](Mode-Line-Top.html)). This variable holds a *mode line construct*: a template that controls what is displayed on the buffer’s mode line. The value of `header-line-format` specifies the buffer’s header line in the same way. All windows for the same buffer use the same `mode-line-format` and `header-line-format` unless a `mode-line-format` or `header-line-format` parameter has been specified for that window (see [Window Parameters](Window-Parameters.html)).

For efficiency, Emacs does not continuously recompute each window’s mode line and header line. It does so when circumstances appear to call for it—for instance, if you change the window configuration, switch buffers, narrow or widen the buffer, scroll, or modify the buffer. If you alter any of the variables referenced by `mode-line-format` or `header-line-format` (see [Mode Line Variables](Mode-Line-Variables.html)), or any other data structures that affect how text is displayed (see [Display](Display.html)), you should use the function `force-mode-line-update` to update the display.

### Function: **force-mode-line-update** *\&optional all*

This function forces Emacs to update the current buffer’s mode line and header line, based on the latest values of all relevant variables, during its next redisplay cycle. If the optional argument `all` is non-`nil`, it forces an update for all mode lines and header lines.

This function also forces an update of the menu bar and frame title.

The selected window’s mode line is usually displayed in a different color using the face `mode-line`. Other windows’ mode lines appear in the face `mode-line-inactive` instead. See [Faces](Faces.html).
