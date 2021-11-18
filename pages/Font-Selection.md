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

Next: [Font Lookup](Font-Lookup.html), Previous: [Basic Faces](Basic-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.12.9 Font Selection

Before Emacs can draw a character on a graphical display, it must select a *font* for that character[23](#FOOT23). See [Fonts](https://www.gnu.org/software/emacs/manual/html_node/emacs/Fonts.html#Fonts) in The GNU Emacs Manual. Normally, Emacs automatically chooses a font based on the faces assigned to that character—specifically, the face attributes `:family`, `:weight`, `:slant`, and `:width` (see [Face Attributes](Face-Attributes.html)). The choice of font also depends on the character to be displayed; some fonts can only display a limited set of characters. If no available font exactly fits the requirements, Emacs looks for the *closest matching font*. The variables in this section control how Emacs makes this selection.

*   User Option: **face-font-family-alternatives**

    If a given family is specified but does not exist, this variable specifies alternative font families to try. Each element should have this form:

        (family alternate-families…)

    If `family` is specified but not available, Emacs will try the other families given in `alternate-families`, one by one, until it finds a family that does exist.

<!---->

*   User Option: **face-font-selection-order**

    If there is no font that exactly matches all desired face attributes (`:width`, `:height`, `:weight`, and `:slant`), this variable specifies the order in which these attributes should be considered when selecting the closest matching font. The value should be a list containing those four attribute symbols, in order of decreasing importance. The default is `(:width :height :weight :slant)`.

    Font selection first finds the best available matches for the first attribute in the list; then, among the fonts which are best in that way, it searches for the best matches in the second attribute, and so on.

    The attributes `:weight` and `:width` have symbolic values in a range centered around `normal`. Matches that are more extreme (farther from `normal`) are somewhat preferred to matches that are less extreme (closer to `normal`); this is designed to ensure that non-normal faces contrast with normal ones, whenever possible.

    One example of a case where this variable makes a difference is when the default font has no italic equivalent. With the default ordering, the `italic` face will use a non-italic font that is similar to the default one. But if you put `:slant` before `:height`, the `italic` face will use an italic font, even if its height is not quite right.

<!---->

*   User Option: **face-font-registry-alternatives**

    This variable lets you specify alternative font registries to try, if a given registry is specified and doesn’t exist. Each element should have this form:

        (registry alternate-registries…)

    If `registry` is specified but not available, Emacs will try the other registries given in `alternate-registries`, one by one, until it finds a registry that does exist.

Emacs can make use of scalable fonts, but by default it does not use them.

*   User Option: **scalable-fonts-allowed**

    This variable controls which scalable fonts to use. A value of `nil`, the default, means do not use scalable fonts. `t` means to use any scalable font that seems appropriate for the text.

    Otherwise, the value must be a list of regular expressions. Then a scalable font is enabled for use if its name matches any regular expression in the list. For example,

        (setq scalable-fonts-allowed '("iso10646-1$"))

    allows the use of scalable fonts with registry `iso10646-1`.

<!---->

*   Variable: **face-font-rescale-alist**

    This variable specifies scaling for certain faces. Its value should be a list of elements of the form

        (fontname-regexp . scale-factor)

    If `fontname-regexp` matches the font name that is about to be used, this says to choose a larger similar font according to the factor `scale-factor`. You would use this feature to normalize the font size if certain fonts are bigger or smaller than their nominal heights and widths would suggest.

***

#### Footnotes

##### [(23)](#DOCF23)

In this context, the term *font* has nothing to do with Font Lock (see [Font Lock Mode](Font-Lock-Mode.html)).

Next: [Font Lookup](Font-Lookup.html), Previous: [Basic Faces](Basic-Faces.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
