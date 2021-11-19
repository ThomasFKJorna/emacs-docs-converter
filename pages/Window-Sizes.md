

Next: [Resizing Windows](Resizing-Windows.html), Previous: [Windows and Frames](Windows-and-Frames.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 28.3 Window Sizes

The following schematic shows the structure of a live window:

```lisp
        ____________________________________________
       |______________ Header Line ______________|RD| ^
     ^ |LS|LM|LF|                       |RF|RM|RS|  | |
     | |  |  |  |                       |  |  |  |  | |
Window |  |  |  |       Text Area       |  |  |  |  | Window
Body | |  |  |  |     (Window Body)     |  |  |  |  | Total
Height |  |  |  |                       |  |  |  |  | Height
     | |  |  |  |<- Window Body Width ->|  |  |  |  | |
     v |__|__|__|_______________________|__|__|__|  | |
       |_________ Horizontal Scroll Bar _________|  | |
       |_______________ Mode Line _______________|__| |
       |_____________ Bottom Divider _______________| v
        <---------- Window Total Width ------------>
```

At the center of the window is the *text area*, or *body*, where the buffer text is displayed. The text area can be surrounded by a series of optional areas. On the left and right, from innermost to outermost, these are the left and right fringes, denoted by LF and RF (see [Fringes](Fringes.html)); the left and right margins, denoted by LM and RM in the schematic (see [Display Margins](Display-Margins.html)); the left or right vertical scroll bar, only one of which is present at any time, denoted by LS and RS (see [Scroll Bars](Scroll-Bars.html)); and the right divider, denoted by RD (see [Window Dividers](Window-Dividers.html)). At the top of the window is the header line (see [Header Lines](Header-Lines.html)). At the bottom of the window are the horizontal scroll bar (see [Scroll Bars](Scroll-Bars.html)); the mode line (see [Mode Line Format](Mode-Line-Format.html)); and the bottom divider (see [Window Dividers](Window-Dividers.html)).

Emacs provides miscellaneous functions for finding the height and width of a window. The return value of many of these functions can be specified either in units of pixels or in units of lines and columns. On a graphical display, the latter actually correspond to the height and width of a default character specified by the frame’s default font as returned by `frame-char-height` and `frame-char-width` (see [Frame Font](Frame-Font.html)). Thus, if a window is displaying text with a different font or size, the reported line height and column width for that window may differ from the actual number of text lines or columns displayed within it.

The *total height* of a window is the number of lines comprising the window’s body, the header line, the horizontal scroll bar, the mode line and the bottom divider (if any).

*   Function: **window-total-height** *\&optional window round*

    This function returns the total height, in lines, of the window `window`. If `window` is omitted or `nil`, it defaults to the selected window. If `window` is an internal window, the return value is the total height occupied by its descendant windows.

    If a window’s pixel height is not an integral multiple of its frame’s default character height, the number of lines occupied by the window is rounded internally. This is done in a way such that, if the window is a parent window, the sum of the total heights of all its child windows internally equals the total height of their parent. This means that although two windows have the same pixel height, their internal total heights may differ by one line. This means also, that if window is vertically combined and has a next sibling, the topmost row of that sibling can be calculated as the sum of this window’s topmost row and total height (see [Coordinates and Windows](Coordinates-and-Windows.html))

    If the optional argument `round` is `ceiling`, this function returns the smallest integer larger than `window`’s pixel height divided by the character height of its frame; if it is `floor`, it returns the largest integer smaller than said value; with any other `round` it returns the internal value of `windows`’s total height.

The *total width* of a window is the number of lines comprising the window’s body, its margins, fringes, scroll bars and a right divider (if any).

*   Function: **window-total-width** *\&optional window round*

    This function returns the total width, in columns, of the window `window`. If `window` is omitted or `nil`, it defaults to the selected window. If `window` is internal, the return value is the total width occupied by its descendant windows.

    If a window’s pixel width is not an integral multiple of its frame’s character width, the number of lines occupied by the window is rounded internally. This is done in a way such that, if the window is a parent window, the sum of the total widths of all its children internally equals the total width of their parent. This means that although two windows have the same pixel width, their internal total widths may differ by one column. This means also, that if this window is horizontally combined and has a next sibling, the leftmost column of that sibling can be calculated as the sum of this window’s leftmost column and total width (see [Coordinates and Windows](Coordinates-and-Windows.html)). The optional argument `round` behaves as it does for `window-total-height`.

<!---->

*   Function: **window-total-size** *\&optional window horizontal round*

    This function returns either the total height in lines or the total width in columns of the window `window`. If `horizontal` is omitted or `nil`, this is equivalent to calling `window-total-height` for `window`; otherwise it is equivalent to calling `window-total-width` for `window`. The optional argument `round` behaves as it does for `window-total-height`.

The following two functions can be used to return the total size of a window in units of pixels.

*   Function: **window-pixel-height** *\&optional window*

    This function returns the total height of window `window` in pixels. `window` must be a valid window and defaults to the selected one.

    The return value includes mode and header line, a horizontal scroll bar and a bottom divider, if any. If `window` is an internal window, its pixel height is the pixel height of the screen areas spanned by its children.

<!---->

*   Function: **window-pixel-width** *\&optional window*

    This function returns the width of window `window` in pixels. `window` must be a valid window and defaults to the selected one.

    The return value includes the fringes and margins of `window` as well as any vertical dividers or scroll bars belonging to `window`. If `window` is an internal window, its pixel width is the width of the screen areas spanned by its children.

The following functions can be used to determine whether a given window has any adjacent windows.

*   Function: **window-full-height-p** *\&optional window*

    This function returns non-`nil` if `window` has no other window above or below it in its frame. More precisely, this means that the total height of `window` equals the total height of the root window on that frame. The minibuffer window does not count in this regard. If `window` is omitted or `nil`, it defaults to the selected window.

<!---->

*   Function: **window-full-width-p** *\&optional window*

    This function returns non-`nil` if `window` has no other window to the left or right in its frame, i.e., its total width equals that of the root window on that frame. If `window` is omitted or `nil`, it defaults to the selected window.

The *body height* of a window is the height of its text area, which does not include a mode or header line, a horizontal scroll bar, or a bottom divider.

*   Function: **window-body-height** *\&optional window pixelwise*

    This function returns the height, in lines, of the body of window `window`. If `window` is omitted or `nil`, it defaults to the selected window; otherwise it must be a live window.

    If the optional argument `pixelwise` is non-`nil`, this function returns the body height of `window` counted in pixels.

    If `pixelwise` is `nil`, the return value is rounded down to the nearest integer, if necessary. This means that if a line at the bottom of the text area is only partially visible, that line is not counted. It also means that the height of a window’s body can never exceed its total height as returned by `window-total-height`.

The *body width* of a window is the width of its text area, which does not include the scroll bar, fringes, margins or a right divider. Note that when one or both fringes are removed (by setting their width to zero), the display engine reserves two character cells, one on each side of the window, for displaying the continuation and truncation glyphs, which leaves 2 columns less for text display. (The function `window-max-chars-per-line`, described below, takes this peculiarity into account.)

*   Function: **window-body-width** *\&optional window pixelwise*

    This function returns the width, in columns, of the body of window `window`. If `window` is omitted or `nil`, it defaults to the selected window; otherwise it must be a live window.

    If the optional argument `pixelwise` is non-`nil`, this function returns the body width of `window` in units of pixels.

    If `pixelwise` is `nil`, the return value is rounded down to the nearest integer, if necessary. This means that if a column on the right of the text area is only partially visible, that column is not counted. It also means that the width of a window’s body can never exceed its total width as returned by `window-total-width`.

<!---->

*   Function: **window-body-size** *\&optional window horizontal pixelwise*

    This function returns the body height or body width of `window`. If `horizontal` is omitted or `nil`, it is equivalent to calling `window-body-height` for `window`; otherwise it is equivalent to calling `window-body-width`. In either case, the optional argument `pixelwise` is passed to the function called.

For compatibility with previous versions of Emacs, `window-height` is an alias for `window-total-height`, and `window-width` is an alias for `window-body-width`. These aliases are considered obsolete and will be removed in the future.

The pixel heights of a window’s mode and header line can be retrieved with the functions given below. Their return value is usually accurate unless the window has not been displayed before: In that case, the return value is based on an estimate of the font used for the window’s frame.

*   Function: **window-mode-line-height** *\&optional window*

    This function returns the height in pixels of `window`’s mode line. `window` must be a live window and defaults to the selected one. If `window` has no mode line, the return value is zero.

<!---->

*   Function: **window-header-line-height** *\&optional window*

    This function returns the height in pixels of `window`’s header line. `window` must be a live window and defaults to the selected one. If `window` has no header line, the return value is zero.

Functions for retrieving the height and/or width of window dividers (see [Window Dividers](Window-Dividers.html)), fringes (see [Fringes](Fringes.html)), scroll bars (see [Scroll Bars](Scroll-Bars.html)), and display margins (see [Display Margins](Display-Margins.html)) are described in the corresponding sections.

If your Lisp program needs to make layout decisions, you will find the following function useful:

*   Function: **window-max-chars-per-line** *\&optional window face*

    This function returns the number of characters displayed in the specified face `face` in the specified window `window` (which must be a live window). If `face` was remapped (see [Face Remapping](Face-Remapping.html)), the information is returned for the remapped face. If omitted or `nil`, `face` defaults to the default face, and `window` defaults to the selected window.

    Unlike `window-body-width`, this function accounts for the actual size of `face`’s font, instead of working in units of the canonical character width of `window`’s frame (see [Frame Font](Frame-Font.html)). It also accounts for space used by the continuation glyph, if `window` lacks one or both of its fringes.

Commands that change the size of windows (see [Resizing Windows](Resizing-Windows.html)), or split them (see [Splitting Windows](Splitting-Windows.html)), obey the variables `window-min-height` and `window-min-width`, which specify the smallest allowable window height and width. They also obey the variable `window-size-fixed`, with which a window can be *fixed* in size (see [Preserving Window Sizes](Preserving-Window-Sizes.html)).

*   User Option: **window-min-height**

    This option specifies the minimum total height, in lines, of any window. Its value has to accommodate at least one text line as well as a mode and header line, a horizontal scroll bar and a bottom divider, if present.

<!---->

*   User Option: **window-min-width**

    This option specifies the minimum total width, in columns, of any window. Its value has to accommodate two text columns as well as margins, fringes, a scroll bar and a right divider, if present.

The following function tells how small a specific window can get taking into account the sizes of its areas and the values of `window-min-height`, `window-min-width` and `window-size-fixed` (see [Preserving Window Sizes](Preserving-Window-Sizes.html)).

*   Function: **window-min-size** *\&optional window horizontal ignore pixelwise*

    This function returns the minimum size of `window`. `window` must be a valid window and defaults to the selected one. The optional argument `horizontal` non-`nil` means to return the minimum number of columns of `window`; otherwise return the minimum number of `window`’s lines.

    The return value makes sure that all components of `window` remain fully visible if `window`’s size were actually set to it. With `horizontal` `nil` it includes the mode and header line, the horizontal scroll bar and the bottom divider, if present. With `horizontal` non-`nil` it includes the margins and fringes, the vertical scroll bar and the right divider, if present.

    The optional argument `ignore`, if non-`nil`, means ignore restrictions imposed by fixed size windows, `window-min-height` or `window-min-width` settings. If `ignore` equals `safe`, live windows may get as small as `window-safe-min-height` lines and `window-safe-min-width` columns. If `ignore` is a window, ignore restrictions for that window only. Any other non-`nil` value means ignore all of the above restrictions for all windows.

    The optional argument `pixelwise` non-`nil` means to return the minimum size of `window` counted in pixels.

Next: [Resizing Windows](Resizing-Windows.html), Previous: [Windows and Frames](Windows-and-Frames.html), Up: [Windows](Windows.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
