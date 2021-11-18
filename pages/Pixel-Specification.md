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

Next: [Other Display Specs](Other-Display-Specs.html), Previous: [Specified Space](Specified-Space.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.16.3 Pixel Specification for Spaces

The value of the `:width`, `:align-to`, `:height`, and `:ascent` properties can be a special kind of expression that is evaluated during redisplay. The result of the evaluation is used as an absolute number of pixels.

The following expressions are supported:

      expr ::= num | (num) | unit | elem | pos | image | xwidget | form
      num  ::= integer | float | symbol
      unit ::= in | mm | cm | width | height

<!---->

      elem ::= left-fringe | right-fringe | left-margin | right-margin
            |  scroll-bar | text
      pos  ::= left | center | right
      form ::= (num . expr) | (op expr ...)
      op   ::= + | -

The form `num` specifies a fraction of the default frame font height or width. The form `(num)` specifies an absolute number of pixels. If `num` is a symbol, `symbol`, its buffer-local variable binding is used; that binding can be either a number or a cons cell of the forms shown above (including yet another cons cell whose `car` is a symbol that has a buffer-local binding).

The `in`, `mm`, and `cm` units specify the number of pixels per inch, millimeter, and centimeter, respectively. The `width` and `height` units correspond to the default width and height of the current face. An image specification of the form `(image . props)`<!-- /@w --> (see [Image Descriptors](Image-Descriptors.html)) corresponds to the width or height of the specified image. Similarly, an xwidget specification of the form `(xwidget . props)`<!-- /@w --> stands for the width or height of the specified xwidget. See [Xwidgets](Xwidgets.html).

The elements `left-fringe`, `right-fringe`, `left-margin`, `right-margin`, `scroll-bar`, and `text` specify the width of the corresponding area of the window. When the window displays line numbers (see [Size of Displayed Text](Size-of-Displayed-Text.html)), the width of the `text` area is decreased by the screen space taken by the line-number display.

The `left`, `center`, and `right` positions can be used with `:align-to` to specify a position relative to the left edge, center, or right edge of the text area. When the window displays line numbers, the `left` and the `center` positions are offset to account for the screen space taken by the line-number display.

Any of the above window elements (except `text`) can also be used with `:align-to` to specify that the position is relative to the left edge of the given area. Once the base offset for a relative position has been set (by the first occurrence of one of these symbols), further occurrences of these symbols are interpreted as the width of the specified area. For example, to align to the center of the left-margin, use

    :align-to (+ left-margin (0.5 . left-margin))

If no specific base offset is set for alignment, it is always relative to the left edge of the text area. For example, ‘`:align-to 0`’ in a header-line aligns with the first text column in the text area. When the window displays line numbers, the text is considered to start where the space used for line-number display ends.

A value of the form `(num . expr)` stands for the product of the values of `num` and `expr`. For example, `(2 . in)` specifies a width of 2 inches, while `(0.5 . image)` specifies half the width (or height) of the specified `image` (which should be given by its image spec).

The form `(+ expr ...)` adds up the value of the expressions. The form `(- expr ...)` negates or subtracts the value of the expressions.

Next: [Other Display Specs](Other-Display-Specs.html), Previous: [Specified Space](Specified-Space.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
