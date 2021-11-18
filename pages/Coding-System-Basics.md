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

Next: [Encoding and I/O](Encoding-and-I_002fO.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 33.10.1 Basic Concepts of Coding Systems

*Character code conversion* involves conversion between the internal representation of characters used inside Emacs and some other encoding. Emacs supports many different encodings, in that it can convert to and from them. For example, it can convert text to or from encodings such as Latin 1, Latin 2, Latin 3, Latin 4, Latin 5, and several variants of ISO 2022. In some cases, Emacs supports several alternative encodings for the same characters; for example, there are three coding systems for the Cyrillic (Russian) alphabet: ISO, Alternativnyj, and KOI8.

Every coding system specifies a particular set of character code conversions, but the coding system `undecided` is special: it leaves the choice unspecified, to be chosen heuristically for each file, based on the file’s data. The coding system `prefer-utf-8` is like `undecided`, but it prefers to choose `utf-8` when possible.

In general, a coding system doesn’t guarantee roundtrip identity: decoding a byte sequence using a coding system, then encoding the resulting text in the same coding system, can produce a different byte sequence. But some coding systems do guarantee that the byte sequence will be the same as what you originally decoded. Here are a few examples:

> iso-8859-1, utf-8, big5, shift\_jis, euc-jp

Encoding buffer text and then decoding the result can also fail to reproduce the original text. For instance, if you encode a character with a coding system which does not support that character, the result is unpredictable, and thus decoding it using the same coding system may produce a different text. Currently, Emacs can’t report errors that result from encoding unsupported characters.

*End of line conversion* handles three different conventions used on various systems for representing end of line in files. The Unix convention, used on GNU and Unix systems, is to use the linefeed character (also called newline). The DOS convention, used on MS-Windows and MS-DOS systems, is to use a carriage return and a linefeed at the end of a line. The Mac convention is to use just carriage return. (This was the convention used in Classic Mac OS.)

*Base coding systems* such as `latin-1` leave the end-of-line conversion unspecified, to be chosen based on the data. *Variant coding systems* such as `latin-1-unix`, `latin-1-dos` and `latin-1-mac` specify the end-of-line conversion explicitly as well. Most base coding systems have three corresponding variants whose names are formed by adding ‘`-unix`’, ‘`-dos`’ and ‘`-mac`’.

The coding system `raw-text` is special in that it prevents character code conversion, and causes the buffer visited with this coding system to be a unibyte buffer. For historical reasons, you can save both unibyte and multibyte text with this coding system. When you use `raw-text` to encode multibyte text, it does perform one character code conversion: it converts eight-bit characters to their single-byte external representation. `raw-text` does not specify the end-of-line conversion, allowing that to be determined as usual by the data, and has the usual three variants which specify the end-of-line conversion.

`no-conversion` (and its alias `binary`) is equivalent to `raw-text-unix`: it specifies no conversion of either character codes or end-of-line.

The coding system `utf-8-emacs` specifies that the data is represented in the internal Emacs encoding (see [Text Representations](Text-Representations.html)). This is like `raw-text` in that no code conversion happens, but different in that the result is multibyte data. The name `emacs-internal` is an alias for `utf-8-emacs-unix` (so it forces no conversion of end-of-line, unlike `utf-8-emacs`, which can decode all 3 kinds of end-of-line conventions).

*   Function: **coding-system-get** *coding-system property*

    This function returns the specified property of the coding system `coding-system`. Most coding system properties exist for internal purposes, but one that you might find useful is `:mime-charset`. That property’s value is the name used in MIME for the character coding which this coding system can read and write. Examples:

        (coding-system-get 'iso-latin-1 :mime-charset)
             ⇒ iso-8859-1
        (coding-system-get 'iso-2022-cn :mime-charset)
             ⇒ iso-2022-cn
        (coding-system-get 'cyrillic-koi8 :mime-charset)
             ⇒ koi8-r

    The value of the `:mime-charset` property is also defined as an alias for the coding system.

<!---->

*   Function: **coding-system-aliases** *coding-system*

    This function returns the list of aliases of `coding-system`.

Next: [Encoding and I/O](Encoding-and-I_002fO.html), Up: [Coding Systems](Coding-Systems.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
