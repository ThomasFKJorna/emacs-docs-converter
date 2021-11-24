

#### 29.3.2 Frame Font

Each frame has a *default font* which specifies the default character size for that frame. This size is meant when retrieving or changing the size of a frame in terms of columns or lines (see [Size Parameters](Size-Parameters.html)). It is also used when resizing (see [Window Sizes](Window-Sizes.html)) or splitting (see [Splitting Windows](Splitting-Windows.html)) windows.

The terms *line height* and *canonical character height* are sometimes used instead of “default character height”. Similarly, the terms *column width* and *canonical character width* are used instead of “default character width”.

*   Function: **frame-char-height** *\&optional frame*
*   Function: **frame-char-width** *\&optional frame*

These functions return the default height and width of a character in `frame`, measured in pixels. Together, these values establish the size of the default font on `frame`. The values depend on the choice of font for `frame`, see [Font and Color Parameters](Font-and-Color-Parameters.html).

The default font can be also set directly with the following function:

### Command: **set-frame-font** *font \&optional keep-size frames*

This sets the default font to `font`. When called interactively, it prompts for the name of a font, and uses that font on the selected frame. When called from Lisp, `font` should be a font name (a string), a font object, font entity, or a font spec.

If the optional argument `keep-size` is `nil`, this keeps the number of frame lines and columns fixed. (If non-`nil`, the option `frame-inhibit-implied-resize` described in the next section will override this.) If `keep-size` is non-`nil` (or with a prefix argument), it tries to keep the size of the display area of the current frame fixed by adjusting the number of lines and columns.

If the optional argument `frames` is `nil`, this applies the font to the selected frame only. If `frames` is non-`nil`, it should be a list of frames to act upon, or `t` meaning all existing and all future graphical frames.
