

Next: [Implied Frame Resizing](Implied-Frame-Resizing.html), Previous: [Frame Position](Frame-Position.html), Up: [Frame Geometry](Frame-Geometry.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 29.3.4 Frame Size

The canonical way to specify the *size of a frame* from within Emacs is by specifying its *text size*—a tuple of the width and height of the frame’s text area (see [Frame Layout](Frame-Layout.html)). It can be measured either in pixels or in terms of the frame’s canonical character size (see [Frame Font](Frame-Font.html)).

For frames with an internal menu or tool bar, the frame’s native height cannot be told exactly before the frame has been actually drawn. This means that in general you cannot use the native size to specify the initial size of a frame. As soon as you know the native size of a visible frame, you can calculate its outer size (see [Frame Layout](Frame-Layout.html)) by adding in the remaining components from the return value of `frame-geometry`. For invisible frames or for frames that have yet to be created, however, the outer size can only be estimated. This also means that calculating an exact initial position of a frame specified via offsets from the right or bottom edge of the screen (see [Frame Position](Frame-Position.html)) is impossible.

The text size of any frame can be set and retrieved with the help of the `height` and `width` frame parameters (see [Size Parameters](Size-Parameters.html)). The text size of the initial frame can be also set with the help of an X-style geometry specification. See [Command Line Arguments for Emacs Invocation](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Invocation.html#Emacs-Invocation) in The GNU Emacs Manual. Below we list some functions to access and set the size of an existing, visible frame, by default the selected one.

*   *   Function: **frame-height** *\&optional frame*
    *   Function: **frame-width** *\&optional frame*

    These functions return the height and width of the text area of `frame`, measured in units of the default font height and width of `frame` (see [Frame Font](Frame-Font.html)). These functions are plain shorthands for writing `(frame-parameter frame 'height)` and `(frame-parameter frame 'width)`.

    If the text area of `frame` measured in pixels is not a multiple of its default font size, the values returned by these functions are rounded down to the number of characters of the default font that fully fit into the text area.

The functions following next return the pixel widths and heights of the native, outer and inner frame and the text area (see [Frame Layout](Frame-Layout.html)) of a given frame. For a text terminal, the results are in characters rather than pixels.

*   *   Function: **frame-outer-width** *\&optional frame*
    *   Function: **frame-outer-height** *\&optional frame*

    These functions return the outer width and height of `frame` in pixels.

<!---->

*   *   Function: **frame-native-height** *\&optional frame*
    *   Function: **frame-native-width** *\&optional frame*

    These functions return the native width and height of `frame` in pixels.

<!---->

*   *   Function: **frame-inner-width** *\&optional frame*
    *   Function: **frame-inner-height** *\&optional frame*

    These functions return the inner width and height of `frame` in pixels.

<!---->

*   *   Function: **frame-text-width** *\&optional frame*
    *   Function: **frame-text-height** *\&optional frame*

    These functions return the width and height of the text area of `frame` in pixels.

On window systems that support it, Emacs tries by default to make the text size of a frame measured in pixels a multiple of the frame’s character size. This, however, usually means that a frame can be resized only in character size increments when dragging its external borders. It also may break attempts to truly maximize the frame or making it “fullheight” or “fullwidth” (see [Size Parameters](Size-Parameters.html)) leaving some empty space below and/or on the right of the frame. The following option may help in that case.

*   User Option: **frame-resize-pixelwise**

    If this option is `nil` (the default), a frame’s text pixel size is usually rounded to a multiple of the current values of that frame’s `frame-char-height` and `frame-char-width` whenever the frame is resized. If this is non-`nil`, no rounding occurs, hence frame sizes can increase/decrease by one pixel.

    Setting this variable usually causes the next resize operation to pass the corresponding size hints to the window manager. This means that this variable should be set only in a user’s initial file; applications should never bind it temporarily.

    The precise meaning of a value of `nil` for this option depends on the toolkit used. Dragging the external border with the mouse is done character-wise provided the window manager is willing to process the corresponding size hints. Calling `set-frame-size` (see below) with arguments that do not specify the frame size as an integer multiple of its character size, however, may: be ignored, cause a rounding (GTK+), or be accepted (Lucid, Motif, MS-Windows).

    With some window managers you may have to set this to non-`nil` in order to make a frame appear truly maximized or full-screen.

<!---->

*   Function: **set-frame-size** *frame width height \&optional pixelwise*

    This function sets the size of the text area of `frame`, measured in terms of the canonical height and width of a character on `frame` (see [Frame Font](Frame-Font.html)).

    The optional argument `pixelwise` non-`nil` means to measure the new width and height in units of pixels instead. Note that if `frame-resize-pixelwise` is `nil`, some toolkits may refuse to truly honor the request if it does not increase/decrease the frame size to a multiple of its character size.

<!---->

*   Function: **set-frame-height** *frame height \&optional pretend pixelwise*

    This function resizes the text area of `frame` to a height of `height` lines. The sizes of existing windows in `frame` are altered proportionally to fit.

    If `pretend` is non-`nil`, then Emacs displays `height` lines of output in `frame`, but does not change its value for the actual height of the frame. This is only useful on text terminals. Using a smaller height than the terminal actually implements may be useful to reproduce behavior observed on a smaller screen, or if the terminal malfunctions when using its whole screen. Setting the frame height directly does not always work, because knowing the correct actual size may be necessary for correct cursor positioning on text terminals.

    The optional fourth argument `pixelwise` non-`nil` means that `frame` should be `height` pixels high. Note that if `frame-resize-pixelwise` is `nil`, some window managers may refuse to truly honor the request if it does not increase/decrease the frame height to a multiple of its character height.

    When used interactively, this command will set the height of the currently selected frame to the number of lines specified by the numeric prefix.

<!---->

*   Function: **set-frame-width** *frame width \&optional pretend pixelwise*

    This function sets the width of the text area of `frame`, measured in characters. The argument `pretend` has the same meaning as in `set-frame-height`.

    The optional fourth argument `pixelwise` non-`nil` means that `frame` should be `width` pixels wide. Note that if `frame-resize-pixelwise` is `nil`, some window managers may refuse to fully honor the request if it does not increase/decrease the frame width to a multiple of its character width.

    When used interactively, this command will set the width of the currently selected frame to the number of columns specified by the numeric prefix.

None of these three functions will make a frame smaller than needed to display all of its windows together with their scroll bars, fringes, margins, dividers, mode and header lines. This contrasts with requests by the window manager triggered, for example, by dragging the external border of a frame with the mouse. Such requests are always honored by clipping, if necessary, portions that cannot be displayed at the right, bottom corner of the frame. The parameters `min-width` and `min-height` (see [Size Parameters](Size-Parameters.html)) can be used to obtain a similar behavior when changing the frame size from within Emacs.

The abnormal hook `window-size-change-functions` (see [Window Hooks](Window-Hooks.html)) tracks all changes of the inner size of a frame including those induced by request of the window-system or window manager. To rule out false positives that might occur when changing only the sizes of a frame’s windows without actually changing the size of the inner frame, use the following function.

*   Function: **frame-size-changed-p** *\&optional frame*

    This function returns non-`nil` when the inner width or height of `frame` has changed since `window-size-change-functions` was run the last time for `frame`. It always returns `nil` immediately after running `window-size-change-functions` for `frame`.

Next: [Implied Frame Resizing](Implied-Frame-Resizing.html), Previous: [Frame Position](Frame-Position.html), Up: [Frame Geometry](Frame-Geometry.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
