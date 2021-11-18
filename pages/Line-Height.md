<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Faces](Faces.html), Previous: [Size of Displayed Text](Size-of-Displayed-Text.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 39.11 Line Height

The total height of each display line consists of the height of the contents of the line, plus optional additional vertical line spacing above or below the display line.

The height of the line contents is the maximum height of any character or image on that display line, including the final newline if there is one. (A display line that is continued doesn’t include a final newline.) That is the default line height, if you do nothing to specify a greater height. (In the most common case, this equals the height of the corresponding frame’s default font, see [Frame Font](Frame-Font.html).)

There are several ways to explicitly specify a larger line height, either by specifying an absolute height for the display line, or by specifying vertical space. However, no matter what you specify, the actual line height can never be less than the default.

A newline can have a `line-height` text or overlay property that controls the total height of the display line ending in that newline. The property value can be one of several forms:

*   `t`

    If the property value is `t`, the newline character has no effect on the displayed height of the line—the visible contents alone determine the height. The `line-spacing` property, described below, is also ignored in this case. This is useful for tiling small images (or image slices) without adding blank areas between the images.

*   `(height total)`

    If the property value is a list of the form shown, that adds extra space *below* the display line. First Emacs uses `height` as a height spec to control extra space *above* the line; then it adds enough space *below* the line to bring the total line height up to `total`. In this case, any value of `line-spacing` property for the newline is ignored.

Any other kind of property value is a height spec, which translates into a number—the specified line height. There are several ways to write a height spec; here’s how each of them translates into a number:

*   `integer`

    If the height spec is a positive integer, the height value is that integer.

*   `float`

    If the height spec is a float, `float`, the numeric height value is `float` times the frame’s default line height.

*   `(face . ratio)`

    If the height spec is a cons of the format shown, the numeric height is `ratio` times the height of face `face`. `ratio` can be any type of number, or `nil` which means a ratio of 1. If `face` is `t`, it refers to the current face.

*   `(nil . ratio)`

    If the height spec is a cons of the format shown, the numeric height is `ratio` times the height of the contents of the line.

Thus, any valid height spec determines the height in pixels, one way or another. If the line contents’ height is less than that, Emacs adds extra vertical space above the line to achieve the specified total height.

If you don’t specify the `line-height` property, the line’s height consists of the contents’ height plus the line spacing. There are several ways to specify the line spacing for different parts of Emacs text.

On graphical terminals, you can specify the line spacing for all lines in a frame, using the `line-spacing` frame parameter (see [Layout Parameters](Layout-Parameters.html)). However, if the default value of `line-spacing` is non-`nil`, it overrides the frame’s `line-spacing` parameter. An integer specifies the number of pixels put below lines. A floating-point number specifies the spacing relative to the frame’s default line height.

You can specify the line spacing for all lines in a buffer via the buffer-local `line-spacing` variable. An integer specifies the number of pixels put below lines. A floating-point number specifies the spacing relative to the default frame line height. This overrides line spacings specified for the frame.

Finally, a newline can have a `line-spacing` text or overlay property that can enlarge the default frame line spacing and the buffer local `line-spacing` variable: if its value is larger than the buffer or frame defaults, that larger value is used instead, for the display line ending in that newline.

One way or another, these mechanisms specify a Lisp value for the spacing of each line. The value is a height spec, and it translates into a Lisp value as described above. However, in this case the numeric height value specifies the line spacing, rather than the line height.

On text terminals, the line spacing cannot be altered.

Next: [Faces](Faces.html), Previous: [Size of Displayed Text](Size-of-Displayed-Text.html), Up: [Display](Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
