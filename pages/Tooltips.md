

Next: [Bidirectional Display](Bidirectional-Display.html), Previous: [Window Systems](Window-Systems.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.25 Tooltips

*Tooltips* are special frames (see [Frames](Frames.html)) that are used to display helpful hints (a.k.a. “tips”) related to the current position of the mouse pointer. Emacs uses tooltips to display help strings about active portions of text (see [Special Properties](Special-Properties.html)) and about various UI elements, such as menu items (see [Extended Menu Items](Extended-Menu-Items.html)) and tool-bar buttons (see [Tool Bar](Tool-Bar.html)).

*   Function: **tooltip-mode**

    Tooltip Mode is a minor mode that enables display of tooltips. Turning off this mode causes the tooltips be displayed in the echo area. On text-mode (a.k.a. “TTY”) frames, tooltips are always displayed in the echo area.

When Emacs is built with GTK+ support, it by default displays tooltips using GTK+ functions, and the appearance of the tooltips is then controlled by GTK+ settings. GTK+ tooltips can be disabled by changing the value of the variable `x-gtk-use-system-tooltips` to `nil`. The rest of this subsection describes how to control non-GTK+ tooltips, which are presented by Emacs itself.

Tooltips are displayed in special frames called tooltip frames, which have their own frame parameters (see [Frame Parameters](Frame-Parameters.html)). Unlike other frames, the default parameters for tooltip frames are stored in a special variable.

*   User Option: **tooltip-frame-parameters**

    This customizable option holds the default frame parameters used for displaying tooltips. Any font and color parameters are ignored, and the corresponding attributes of the `tooltip` face are used instead. If `left` or `top` parameters are included, they are used as absolute frame-relative coordinates where the tooltip should be shown. (Mouse-relative position of the tooltip can be customized using the variables described in [Tooltips](https://www.gnu.org/software/emacs/manual/html_node/emacs/Tooltips.html#Tooltips) in The GNU Emacs Manual.) Note that the `left` and `top` parameters, if present, override the values of mouse-relative offsets.

The `tooltip` face determines the appearance of text shown in tooltips. It should generally use a variable-pitch font of size that is preferably smaller than the default frame font.

*   Variable: **tooltip-functions**

    This abnormal hook is a list of functions to call when Emacs needs to display a tooltip. Each function is called with a single argument `event` which is a copy of the last mouse movement event. If a function on this list actually displays the tooltip, it should return non-`nil`, and then the rest of the functions will not be called. The default value of this variable is a single function `tooltip-help-tips`.

If you write your own function to be put on the `tooltip-functions` list, you may need to know the buffer of the mouse event that triggered the tooltip display. The following function provides that information.

*   Function: **tooltip-event-buffer** *event*

    This function returns the buffer over which `event` occurred. Call it with the argument of the function from `tooltip-functions` to obtain the buffer whose text triggered the tooltip. Note that the event might occur not over a buffer (e.g., over the tool bar), in which case this function will return `nil`.

Other aspects of tooltip display are controlled by several customizable settings; see [Tooltips](https://www.gnu.org/software/emacs/manual/html_node/emacs/Tooltips.html#Tooltips) in The GNU Emacs Manual.

Next: [Bidirectional Display](Bidirectional-Display.html), Previous: [Window Systems](Window-Systems.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
