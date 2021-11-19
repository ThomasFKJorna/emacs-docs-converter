

Previous: [Frame Size](Frame-Size.html), Up: [Frame Geometry](Frame-Geometry.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 29.3.5 Implied Frame Resizing

By default, Emacs tries to keep the number of lines and columns of a frame’s text area unaltered when, for example, toggling its menu or tool bar, changing its default font or setting the width of any of its scroll bars. This means that in such case Emacs must ask the window manager to resize the frame’s window in order to accommodate the size change.

Occasionally, such *implied frame resizing* may be unwanted, for example, when a frame has been maximized or made full-screen (where it’s turned off by default). In general, users can disable implied resizing with the following option:

*   User Option: **frame-inhibit-implied-resize**

    If this option is `nil`, changing a frame’s font, menu bar, tool bar, internal borders, fringes or scroll bars may resize its outer frame in order to keep the number of columns or lines of its text area unaltered. If this option is `t`, no such resizing is done.

    The value of this option can be also a list of frame parameters. In that case, implied resizing is inhibited for the change of a parameter that appears in this list. Parameters currently handled by this option are `font`, `font-backend`, `internal-border-width`, `menu-bar-lines` and `tool-bar-lines`.

    Changing any of the `scroll-bar-width`, `scroll-bar-height`, `vertical-scroll-bars`, `horizontal-scroll-bars`, `left-fringe` and `right-fringe` frame parameters is handled as if the frame contained just one live window. This means, for example, that removing vertical scroll bars on a frame containing several side by side windows will shrink the outer frame width by the width of one scroll bar provided this option is `nil` and keep it unchanged if this option is `t` or a list containing `vertical-scroll-bars`.

    The default value is `'(tab-bar-lines tool-bar-lines)` for Lucid, Motif and MS-Windows (which means that adding/removing a tool or tab bar there does not change the outer frame height), `'(tab-bar-lines)` on all other window systems including GTK+ (which means that changing any of the parameters listed above with the exception of `tab-bar-lines` may change the size of the outer frame), and `t` otherwise (which means the outer frame size never changes implicitly when there’s no window system support).

    Note that when a frame is not large enough to accommodate a change of any of the parameters listed above, Emacs may try to enlarge the frame even if this option is non-`nil`.

    Note also that window managers usually do not ask for resizing a frame when they change the number of lines occupied by an external menu or tool bar. Typically, such “wrappings” occur when a user shrinks a frame horizontally, making it impossible to display all elements of its menu or tool bar. They may also result from a change of the major mode altering the number of items of a menu or tool bar. Any such wrappings may implicitly alter the number of lines of a frame’s text area and are unaffected by the setting of this option.

Previous: [Frame Size](Frame-Size.html), Up: [Frame Geometry](Frame-Geometry.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
