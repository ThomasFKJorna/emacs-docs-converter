

Next: [Line Height](Line-Height.html), Previous: [Overlays](Overlays.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.10 Size of Displayed Text

Since not all characters have the same width, these functions let you check the width of a character. See [Primitive Indent](Primitive-Indent.html), and [Screen Lines](Screen-Lines.html), for related functions.

*   Function: **char-width** *char*

    This function returns the width in columns of the character `char`, if it were displayed in the current buffer (i.e., taking into account the buffer’s display table, if any; see [Display Tables](Display-Tables.html)). The width of a tab character is usually `tab-width` (see [Usual Display](Usual-Display.html)).

<!---->

*   Function: **string-width** *string*

    This function returns the width in columns of the string `string`, if it were displayed in the current buffer and the selected window.

<!---->

*   Function: **truncate-string-to-width** *string width \&optional start-column padding ellipsis*

    This function returns the part of `string` that fits within `width` columns, as a new string.

    If `string` does not reach `width`, then the result ends where `string` ends. If one multi-column character in `string` extends across the column `width`, that character is not included in the result. Thus, the result can fall short of `width` but cannot go beyond it.

    The optional argument `start-column` specifies the starting column. If this is non-`nil`, then the first `start-column` columns of the string are omitted from the value. If one multi-column character in `string` extends across the column `start-column`, that character is not included.

    The optional argument `padding`, if non-`nil`, is a padding character added at the beginning and end of the result string, to extend it to exactly `width` columns. The padding character is used at the end of the result if it falls short of `width`. It is also used at the beginning of the result if one multi-column character in `string` extends across the column `start-column`.

    If `ellipsis` is non-`nil`, it should be a string which will replace the end of `string` (including any padding) if it extends beyond `width`, unless the display width of `string` is equal to or less than the display width of `ellipsis`. If `ellipsis` is non-`nil` and not a string, it stands for the value of the variable `truncate-string-ellipsis`.

    ```lisp
    (truncate-string-to-width "\tab\t" 12 4)
         ⇒ "ab"
    (truncate-string-to-width "\tab\t" 12 4 ?\s)
         ⇒ "    ab  "
    ```

The following function returns the size in pixels of text as if it were displayed in a given window. This function is used by `fit-window-to-buffer` and `fit-frame-to-buffer` (see [Resizing Windows](Resizing-Windows.html)) to make a window exactly as large as the text it contains.

*   Function: **window-text-pixel-size** *\&optional window from to x-limit y-limit mode-and-header-line*

    This function returns the size of the text of `window`’s buffer in pixels. `window` must be a live window and defaults to the selected one. The return value is a cons of the maximum pixel-width of any text line and the maximum pixel-height of all text lines. This function exists to allow Lisp programs to adjust the dimensions of `window` to the buffer text it needs to display.

    The optional argument `from`, if non-`nil`, specifies the first text position to consider, and defaults to the minimum accessible position of the buffer. If `from` is `t`, it stands for the minimum accessible position that is not a newline character. The optional argument `to`, if non-`nil`, specifies the last text position to consider, and defaults to the maximum accessible position of the buffer. If `to` is `t`, it stands for the maximum accessible position that is not a newline character.

    The optional argument `x-limit`, if non-`nil`, specifies the maximum X coordinate beyond which text should be ignored; it is therefore also the largest value of pixel-width that the function can return. If `x-limit` `nil` or omitted, it means to use the pixel-width of `window`’s body (see [Window Sizes](Window-Sizes.html)); this default means that text of truncated lines wider than the window will be ignored. This default is useful when the caller does not intend to change the width of `window`. Otherwise, the caller should specify here the maximum width `window`’s body may assume; in particular, if truncated lines are expected and their text needs to be accounted for, `x-limit` should be set to a large value. Since calculating the width of long lines can take some time, it’s always a good idea to make this argument as small as needed; in particular, if the buffer might contain long lines that will be truncated anyway.

    The optional argument `y-limit`, if non-`nil`, specifies the maximum Y coordinate beyond which text is to be ignored; it is therefore also the maximum pixel-height that the function can return. If `y-limit` is nil or omitted, it means to considers all the lines of text till the buffer position specified by `to`. Since calculating the pixel-height of a large buffer can take some time, it makes sense to specify this argument; in particular, if the caller does not know the size of the buffer.

    The optional argument `mode-and-header-line` `nil` or omitted means to not include the height of the mode- or header-line of `window` in the return value. If it is either the symbol `mode-line` or `header-line`, include only the height of that line, if present, in the return value. If it is `t`, include the height of both, if present, in the return value.

`window-text-pixel-size` treats the text displayed in a window as a whole and does not care about the size of individual lines. The following function does.

*   Function: **window-lines-pixel-dimensions** *\&optional window first last body inverse left*

    This function calculates the pixel dimensions of each line displayed in the specified `window`. It does so by walking `window`’s current glyph matrix—a matrix storing the glyph (see [Glyphs](Glyphs.html)) of each buffer character currently displayed in `window`. If successful, it returns a list of cons pairs representing the x- and y-coordinates of the lower right corner of the last character of each line. Coordinates are measured in pixels from an origin (0, 0) at the top-left corner of `window`. `window` must be a live window and defaults to the selected one.

    If the optional argument `first` is an integer, it denotes the index (starting with 0) of the first line of `window`’s glyph matrix to be returned. Note that if `window` has a header line, the line with index 0 is that header line. If `first` is `nil`, the first line to be considered is determined by the value of the optional argument `body`: If `body` is non-`nil`, this means to start with the first line of `window`’s body, skipping any header line, if present. Otherwise, this function will start with the first line of `window`’s glyph matrix, possibly the header line.

    If the optional argument `last` is an integer, it denotes the index of the last line of `window`’s glyph matrix that shall be returned. If `last` is `nil`, the last line to be considered is determined by the value of `body`: If `body` is non-`nil`, this means to use the last line of `window`’s body, omitting `window`’s mode line, if present. Otherwise, this means to use the last line of `window` which may be the mode line.

    The optional argument `inverse`, if `nil`, means that the y-pixel value returned for any line specifies the distance in pixels from the left edge (body edge if `body` is non-`nil`) of `window` to the right edge of the last glyph of that line. `inverse` non-`nil` means that the y-pixel value returned for any line specifies the distance in pixels from the right edge of the last glyph of that line to the right edge (body edge if `body` is non-`nil`) of `window`. This is useful for determining the amount of slack space at the end of each line.

    The optional argument `left`, if non-`nil` means to return the x- and y-coordinates of the lower left corner of the leftmost character on each line. This is the value that should be used for windows that mostly display text from right to left.

    If `left` is non-`nil` and `inverse` is `nil`, this means that the y-pixel value returned for any line specifies the distance in pixels from the left edge of the last (leftmost) glyph of that line to the right edge (body edge if `body` is non-`nil`) of `window`. If `left` and `inverse` are both non-`nil`, the y-pixel value returned for any line specifies the distance in pixels from the left edge (body edge if `body` is non-`nil`) of `window` to the left edge of the last (leftmost) glyph of that line.

    This function returns `nil` if the current glyph matrix of `window` is not up-to-date which usually happens when Emacs is busy, for example, when processing a command. The value should be retrievable though when this function is run from an idle timer with a delay of zero seconds.

<!---->

*   Function: **line-pixel-height**

    This function returns the height in pixels of the line at point in the selected window. The value includes the line spacing of the line (see [Line Height](Line-Height.html)).

When a buffer is displayed with line numbers (see [Display Custom](https://www.gnu.org/software/emacs/manual/html_node/emacs/Display-Custom.html#Display-Custom) in The GNU Emacs Manual), it is sometimes useful to know the width taken for displaying the line numbers. The following function is for Lisp programs which need this information for layout calculations.

*   Function: **line-number-display-width** *\&optional pixelwise*

    This function returns the width used for displaying the line numbers in the selected window. If the optional argument `pixelwise` is the symbol `columns`, the return value is a float number of the frame’s canonical columns; if `pixelwise` is `t` or any other non-`nil` value, the value is an integer and is measured in pixels. If `pixelwise` is omitted or `nil`, the value is the integer number of columns of the font defined for the `line-number` face, and doesn’t include the 2 columns used to pad the numbers on display. If line numbers are not displayed in the selected window, the value is zero regardless of the value of `pixelwise`. Use `with-selected-window` (see [Selecting Windows](Selecting-Windows.html)) if you need this information about another window.

Next: [Line Height](Line-Height.html), Previous: [Overlays](Overlays.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
