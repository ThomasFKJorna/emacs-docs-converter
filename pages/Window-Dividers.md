

Next: [Display Property](Display-Property.html), Previous: [Scroll Bars](Scroll-Bars.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.15 Window Dividers

Window dividers are bars drawn between a frame’s windows. A right divider is drawn between a window and any adjacent windows on the right. Its width (thickness) is specified by the frame parameter `right-divider-width`. A bottom divider is drawn between a window and adjacent windows on the bottom or the echo area. Its width is specified by the frame parameter `bottom-divider-width`. In either case, specifying a width of zero means to not draw such dividers. See [Layout Parameters](Layout-Parameters.html).

Technically, a right divider belongs to the window on its left, which means that its width contributes to the total width of that window. A bottom divider belongs to the window above it, which means that its width contributes to the total height of that window. See [Window Sizes](Window-Sizes.html). When a window has both, a right and a bottom divider, the bottom divider prevails. This means that a bottom divider is drawn over the full total width of its window while the right divider ends above the bottom divider.

Dividers can be dragged with the mouse and are therefore useful for adjusting the sizes of adjacent windows with the mouse. They also serve to visually set apart adjacent windows when no scroll bars or mode lines are present. The following three faces allow the customization of the appearance of dividers:

*   `window-divider`

    When a divider is less than three pixels wide, it is drawn solidly with the foreground of this face. For larger dividers this face is used for the inner part only, excluding the first and last pixel.

*   `window-divider-first-pixel`

    This is the face used for drawing the first pixel of a divider that is at least three pixels wide. To obtain a solid appearance, set this to the same value used for the `window-divider` face.

*   `window-divider-last-pixel`

    This is the face used for drawing the last pixel of a divider that is at least three pixels wide. To obtain a solid appearance, set this to the same value used for the `window-divider` face.

You can get the sizes of the dividers of a specific window with the following two functions.

*   Function: **window-right-divider-width** *\&optional window*

    Return the width (thickness) in pixels of `window`’s right divider. `window` must be a live window and defaults to the selected one. The return value is always zero for a rightmost window.

<!---->

*   Function: **window-bottom-divider-width** *\&optional window*

    Return the width (thickness) in pixels of `window`’s bottom divider. `window` must be a live window and defaults to the selected one. The return value is zero for the minibuffer window or a bottommost window on a minibuffer-less frame.

Next: [Display Property](Display-Property.html), Previous: [Scroll Bars](Scroll-Bars.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
