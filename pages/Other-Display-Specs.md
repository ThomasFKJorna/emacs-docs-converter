

Next: [Display Margins](Display-Margins.html), Previous: [Pixel Specification](Pixel-Specification.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.16.4 Other Display Specifications

Here are the other sorts of display specifications that you can use in the `display` text property.

*   `string`

    Display `string` instead of the text that has this property.

    Recursive display specifications are not supported—`string`’s `display` properties, if any, are not used.

*   `(image . image-props)`

    This kind of display specification is an image descriptor (see [Image Descriptors](Image-Descriptors.html)). When used as a display specification, it means to display the image instead of the text that has the display specification.

*   `(slice x y width height)`

    This specification together with `image` specifies a *slice* (a partial area) of the image to display. The elements `y` and `x` specify the top left corner of the slice, within the image; `width` and `height` specify the width and height of the slice. Integers are numbers of pixels. A floating-point number in the range 0.0–1.0 stands for that fraction of the width or height of the entire image.

*   `((margin nil) string)`

    A display specification of this form means to display `string` instead of the text that has the display specification, at the same position as that text. It is equivalent to using just `string`, but it is done as a special case of marginal display (see [Display Margins](Display-Margins.html)).

*   *   `(left-fringe bitmap [face])`
    *   `(right-fringe bitmap [face])`

    This display specification on any character of a line of text causes the specified `bitmap` be displayed in the left or right fringes for that line, instead of the characters that have the display specification. The optional `face` specifies the face whose colors are to be used for the bitmap display. See [Fringe Bitmaps](Fringe-Bitmaps.html), for the details.

*   `(space-width factor)`

    This display specification affects all the space characters within the text that has the specification. It displays all of these spaces `factor` times as wide as normal. The element `factor` should be an integer or float. Characters other than spaces are not affected at all; in particular, this has no effect on tab characters.

*   `(height height)`

    This display specification makes the text taller or shorter. Here are the possibilities for `height`:

    *   `(+ n)`

        This means to use a font that is `n` steps larger. A *step* is defined by the set of available fonts—specifically, those that match what was otherwise specified for this text, in all attributes except height. Each size for which a suitable font is available counts as another step. `n` should be an integer.

    *   `(- n)`

        This means to use a font that is `n` steps smaller.

    *   a number, `factor`

        A number, `factor`, means to use a font that is `factor` times as tall as the default font.

    *   a symbol, `function`

        A symbol is a function to compute the height. It is called with the current height as argument, and should return the new height to use.

    *   anything else, `form`

        If the `height` value doesn’t fit the previous possibilities, it is a form. Emacs evaluates it to get the new height, with the symbol `height` bound to the current specified font height.

*   `(raise factor)`

    This kind of display specification raises or lowers the text it applies to, relative to the baseline of the line. It is mainly meant to support display of subscripts and superscripts.

    The `factor` must be a number, which is interpreted as a multiple of the height of the affected text. If it is positive, that means to display the characters raised. If it is negative, that means to display them lower down.

    Note that if the text also has a `height` display specification, which was specified before (i.e. to the left of) `raise`, the latter will affect the amount of raising or lowering in pixels, because that is based on the height of the text being raised. Therefore, if you want to display a sub- or superscript that is smaller than the normal text height, consider specifying `raise` before `height`.

You can make any display specification conditional. To do that, package it in another list of the form `(when condition . spec)`. Then the specification `spec` applies only when `condition` evaluates to a non-`nil` value. During the evaluation, `object` is bound to the string or buffer having the conditional `display` property. `position` and `buffer-position` are bound to the position within `object` and the buffer position where the `display` property was found, respectively. Both positions can be different when `object` is a string.

Note that `condition` will only be evaluated when redisplay examines the text where this display spec is located, so this feature is best suited for conditions that are relatively stable, i.e. yield, for each particular buffer position, the same results on every evaluation. If the results change for the same text location, e.g., if the result depends on the position of point, then the conditional specification might not do what you want, because redisplay examines only those parts of buffer text where it has reasons to assume that something changed since the last display cycle.

Next: [Display Margins](Display-Margins.html), Previous: [Pixel Specification](Pixel-Specification.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
