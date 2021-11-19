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

Next: [Pixel Specification](Pixel-Specification.html), Previous: [Replacing Specs](Replacing-Specs.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.16.2 Specified Spaces

To display a space of specified width and/or height, use a display specification of the form `(space . props)`, where `props` is a property list (a list of alternating properties and values). You can put this property on one or more consecutive characters; a space of the specified height and width is displayed in place of *all* of those characters. These are the properties you can use in `props` to specify the weight of the space:

*   `:width width`

    If `width` is a number, it specifies that the space width should be `width` times the normal character width. `width` can also be a *pixel width* specification (see [Pixel Specification](Pixel-Specification.html)).

*   `:relative-width factor`

    Specifies that the width of the stretch should be computed from the first character in the group of consecutive characters that have the same `display` property. The space width is the pixel width of that character, multiplied by `factor`. (On text-mode terminals, the “pixel width” of a character is usually 1, but it could be more for TABs and double-width CJK characters.)

*   `:align-to hpos`

    Specifies that the space should be wide enough to reach `hpos`. If `hpos` is a number, it is measured in units of the normal character width. `hpos` can also be a *pixel width* specification (see [Pixel Specification](Pixel-Specification.html)).

You should use one and only one of the above properties. You can also specify the height of the space, with these properties:

*   `:height height`

    Specifies the height of the space. If `height` is a number, it specifies that the space height should be `height` times the normal character height. The `height` may also be a *pixel height* specification (see [Pixel Specification](Pixel-Specification.html)).

*   `:relative-height factor`

    Specifies the height of the space, multiplying the ordinary height of the text having this display specification by `factor`.

*   `:ascent ascent`

    If the value of `ascent` is a non-negative number no greater than 100, it specifies that `ascent` percent of the height of the space should be considered as the ascent of the space—that is, the part above the baseline. The ascent may also be specified in pixel units with a *pixel ascent* specification (see [Pixel Specification](Pixel-Specification.html)).

Don’t use both `:height` and `:relative-height` together.

The `:width` and `:align-to` properties are supported on non-graphic terminals, but the other space properties in this section are not.

Note that space properties are treated as paragraph separators for the purposes of reordering bidirectional text for display. See [Bidirectional Display](Bidirectional-Display.html), for the details.

Next: [Pixel Specification](Pixel-Specification.html), Previous: [Replacing Specs](Replacing-Specs.html), Up: [Display Property](Display-Property.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
