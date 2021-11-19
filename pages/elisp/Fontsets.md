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

Next: [Low-Level Font](Low_002dLevel-Font.html), Previous: [Font Lookup](Font-Lookup.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.12.11 Fontsets

A *fontset* is a list of fonts, each assigned to a range of character codes. An individual font cannot display the whole range of characters that Emacs supports, but a fontset can. Fontsets have names, just as fonts do, and you can use a fontset name in place of a font name when you specify the font for a frame or a face. Here is information about defining a fontset under Lisp program control.

*   Function: **create-fontset-from-fontset-spec** *fontset-spec \&optional style-variant-p noerror*

    This function defines a new fontset according to the specification string `fontset-spec`. The string should have this format:

        fontpattern, [charset:font]…

    Whitespace characters before and after the commas are ignored.

    The first part of the string, `fontpattern`, should have the form of a standard X font name, except that the last two fields should be ‘`fontset-alias`’.

    The new fontset has two names, one long and one short. The long name is `fontpattern` in its entirety. The short name is ‘`fontset-alias`’. You can refer to the fontset by either name. If a fontset with the same name already exists, an error is signaled, unless `noerror` is non-`nil`, in which case this function does nothing.

    If optional argument `style-variant-p` is non-`nil`, that says to create bold, italic and bold-italic variants of the fontset as well. These variant fontsets do not have a short name, only a long one, which is made by altering `fontpattern` to indicate the bold and/or italic status.

    The specification string also says which fonts to use in the fontset. See below for the details.

The construct ‘`charset:font`’ specifies which font to use (in this fontset) for one particular character set. Here, `charset` is the name of a character set, and `font` is the font to use for that character set. You can use this construct any number of times in the specification string.

For the remaining character sets, those that you don’t specify explicitly, Emacs chooses a font based on `fontpattern`: it replaces ‘`fontset-alias`’ with a value that names one character set. For the ASCII character set, ‘`fontset-alias`’ is replaced with ‘`ISO8859-1`’.

In addition, when several consecutive fields are wildcards, Emacs collapses them into a single wildcard. This is to prevent use of auto-scaled fonts. Fonts made by scaling larger fonts are not usable for editing, and scaling a smaller font is not useful because it is better to use the smaller font in its own size, which Emacs does.

Thus if `fontpattern` is this,

    -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24

the font specification for ASCII characters would be this:

    -*-fixed-medium-r-normal-*-24-*-ISO8859-1

and the font specification for Chinese GB2312 characters would be this:

    -*-fixed-medium-r-normal-*-24-*-gb2312*-*

You may not have any Chinese font matching the above font specification. Most X distributions include only Chinese fonts that have ‘`song ti`’ or ‘`fangsong ti`’ in the `family` field. In such a case, ‘`Fontset-n`’ can be specified as below:

    Emacs.Fontset-0: -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24,\
            chinese-gb2312:-*-*-medium-r-normal-*-24-*-gb2312*-*

Then, the font specifications for all but Chinese GB2312 characters have ‘`fixed`’ in the `family` field, and the font specification for Chinese GB2312 characters has a wild card ‘`*`’ in the `family` field.

*   Function: **set-fontset-font** *name character font-spec \&optional frame add*

    This function modifies the existing fontset `name` to use the font matching with `font-spec` for the specified `character`.

    If `name` is `nil`, this function modifies the fontset of the selected frame or that of `frame` if `frame` is not `nil`.

    If `name` is `t`, this function modifies the default fontset, whose short name is ‘`fontset-default`’.

    In addition to specifying a single codepoint, `character` may be a cons `(from . to)`, where `from` and `to` are character codepoints. In that case, use `font-spec` for all the characters in the range `from` and `to` (inclusive).

    `character` may be a charset (see [Character Sets](Character-Sets.html)). In that case, use `font-spec` for all the characters in the charset.

    `character` may be a script name (see [char-script-table](Character-Properties.html)). In that case, use `font-spec` for all the characters belonging to the script.

    `character` may be `nil`, which means to use `font-spec` for any character which no font-spec is specified.

    `font-spec` may be a font-spec object created by the function `font-spec` (see [Low-Level Font](Low_002dLevel-Font.html)).

    `font-spec` may be a cons; `(family . registry)`, where `family` is a family name of a font (possibly including a foundry name at the head), `registry` is a registry name of a font (possibly including an encoding name at the tail).

    `font-spec` may be a font name, a string.

    `font-spec` may be `nil`, which explicitly specifies that there’s no font for the specified `character`. This is useful, for example, to avoid expensive system-wide search for fonts for characters that have no glyphs, like those from the Unicode Private Use Area (PUA).

    The optional argument `add`, if non-`nil`, specifies how to add `font-spec` to the font specifications previously set. If it is `prepend`, `font-spec` is prepended. If it is `append`, `font-spec` is appended. By default, `font-spec` overrides the previous settings.

    For instance, this changes the default fontset to use a font of which family name is ‘`Kochi Gothic`’ for all characters belonging to the charset `japanese-jisx0208`.

        (set-fontset-font t 'japanese-jisx0208
                          (font-spec :family "Kochi Gothic"))

<!---->

*   Function: **char-displayable-p** *char*

    This function returns `t` if Emacs ought to be able to display `char`. More precisely, if the selected frame’s fontset has a font to display the character set that `char` belongs to.

    Fontsets can specify a font on a per-character basis; when the fontset does that, this function’s value may not be accurate.

Next: [Low-Level Font](Low_002dLevel-Font.html), Previous: [Font Lookup](Font-Lookup.html), Up: [Faces](Faces.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
