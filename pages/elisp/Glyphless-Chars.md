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

Previous: [Glyphs](Glyphs.html), Up: [Character Display](Character-Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 39.22.5 Glyphless Character Display

*Glyphless characters* are characters which are displayed in a special way, e.g., as a box containing a hexadecimal code, instead of being displayed literally. These include characters which are explicitly defined to be glyphless, as well as characters for which there is no available font (on a graphical display), and characters which cannot be encoded by the terminal’s coding system (on a text terminal).

*   Variable: **glyphless-char-display**

    The value of this variable is a char-table which defines glyphless characters and how they are displayed. Each entry must be one of the following display methods:

    *   `nil`

        Display the character in the usual way.

    *   `zero-width`

        Don’t display the character.

    *   `thin-space`

        Display a thin space, 1-pixel wide on graphical displays, or 1-character wide on text terminals.

    *   `empty-box`

        Display an empty box.

    *   `hex-code`

        Display a box containing the Unicode codepoint of the character, in hexadecimal notation.

    *   an ASCII string

        Display a box containing that string. The string should contain at most 6 ASCII characters.

    *   a cons cell `(graphical . text)`

        Display with `graphical` on graphical displays, and with `text` on text terminals. Both `graphical` and `text` must be one of the display methods described above.

    The `thin-space`, `empty-box`, `hex-code`, and ASCII string display methods are drawn with the `glyphless-char` face. On text terminals, a box is emulated by square brackets, ‘`[]`’.

    The char-table has one extra slot, which determines how to display any character that cannot be displayed with any available font, or cannot be encoded by the terminal’s coding system. Its value should be one of the above display methods, except `zero-width` or a cons cell.

    If a character has a non-`nil` entry in an active display table, the display table takes effect; in this case, Emacs does not consult `glyphless-char-display` at all.

<!---->

*   User Option: **glyphless-char-display-control**

    This user option provides a convenient way to set `glyphless-char-display` for groups of similar characters. Do not set its value directly from Lisp code; the value takes effect only via a custom `:set` function (see [Variable Definitions](Variable-Definitions.html)), which updates `glyphless-char-display`.

    Its value should be an alist of elements `(group . method)`, where `group` is a symbol specifying a group of characters, and `method` is a symbol specifying how to display them.

    `group` should be one of the following:

    *   `c0-control`

        ASCII control characters `U+0000` to `U+001F`, excluding the newline and tab characters (normally displayed as escape sequences like ‘`^A`’; see [How Text Is Displayed](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Display.html#Text-Display) in The GNU Emacs Manual).

    *   `c1-control`

        Non-ASCII, non-printing characters `U+0080` to `U+009F` (normally displayed as octal escape sequences like ‘`\230`’).

    *   `format-control`

        Characters of Unicode General Category \[Cf], such as U+200E LEFT-TO-RIGHT MARK, but excluding characters that have graphic images, such as U+00AD SOFT HYPHEN.

    *   `no-font`

        Characters for which there is no suitable font, or which cannot be encoded by the terminal’s coding system.

    The `method` symbol should be one of `zero-width`, `thin-space`, `empty-box`, or `hex-code`. These have the same meanings as in `glyphless-char-display`, above.

Previous: [Glyphs](Glyphs.html), Up: [Character Display](Character-Display.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
