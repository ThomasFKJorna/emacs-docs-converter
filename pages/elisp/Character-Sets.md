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

Next: [Scanning Charsets](Scanning-Charsets.html), Previous: [Character Properties](Character-Properties.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.7 Character Sets

An Emacs *character set*, or *charset*, is a set of characters in which each character is assigned a numeric code point. (The Unicode Standard calls this a *coded character set*.) Each Emacs charset has a name which is a symbol. A single character can belong to any number of different character sets, but it will generally have a different code point in each charset. Examples of character sets include `ascii`, `iso-8859-1`, `greek-iso8859-7`, and `windows-1255`. The code point assigned to a character in a charset is usually different from its code point used in Emacs buffers and strings.

Emacs defines several special character sets. The character set `unicode` includes all the characters whose Emacs code points are in the range `0..#x10FFFF`. The character set `emacs` includes all ASCII and non-ASCII characters. Finally, the `eight-bit` charset includes the 8-bit raw bytes; Emacs uses it to represent raw bytes encountered in text.

*   Function: **charsetp** *object*

    Returns `t` if `object` is a symbol that names a character set, `nil` otherwise.

<!---->

*   Variable: **charset-list**

    The value is a list of all defined character set names.

<!---->

*   Function: **charset-priority-list** *\&optional highestp*

    This function returns a list of all defined character sets ordered by their priority. If `highestp` is non-`nil`, the function returns a single character set of the highest priority.

<!---->

*   Function: **set-charset-priority** *\&rest charsets*

    This function makes `charsets` the highest priority character sets.

<!---->

*   Function: **char-charset** *character \&optional restriction*

    This function returns the name of the character set of highest priority that `character` belongs to. ASCII characters are an exception: for them, this function always returns `ascii`.

    If `restriction` is non-`nil`, it should be a list of charsets to search. Alternatively, it can be a coding system, in which case the returned charset must be supported by that coding system (see [Coding Systems](Coding-Systems.html)).

<!---->

*   Function: **charset-plist** *charset*

    This function returns the property list of the character set `charset`. Although `charset` is a symbol, this is not the same as the property list of that symbol. Charset properties include important information about the charset, such as its documentation string, short name, etc.

<!---->

*   Function: **put-charset-property** *charset propname value*

    This function sets the `propname` property of `charset` to the given `value`.

<!---->

*   Function: **get-charset-property** *charset propname*

    This function returns the value of `charset`s property `propname`.

<!---->

*   Command: **list-charset-chars** *charset*

    This command displays a list of characters in the character set `charset`.

Emacs can convert between its internal representation of a character and the character’s codepoint in a specific charset. The following two functions support these conversions.

*   Function: **decode-char** *charset code-point*

    This function decodes a character that is assigned a `code-point` in `charset`, to the corresponding Emacs character, and returns it. If `charset` doesn’t contain a character of that code point, the value is `nil`.

    For backward compatibility, if `code-point` doesn’t fit in a Lisp fixnum (see [most-positive-fixnum](Integer-Basics.html)), it can be specified as a cons cell `(high . low)`, where `low` are the lower 16 bits of the value and `high` are the high 16 bits. This usage is obsolescent.

<!---->

*   Function: **encode-char** *char charset*

    This function returns the code point assigned to the character `char` in `charset`. If `charset` doesn’t have a codepoint for `char`, the value is `nil`.

The following function comes in handy for applying a certain function to all or part of the characters in a charset:

*   Function: **map-charset-chars** *function charset \&optional arg from-code to-code*

    Call `function` for characters in `charset`. `function` is called with two arguments. The first one is a cons cell `(from . to)`, where `from` and `to` indicate a range of characters contained in charset. The second argument passed to `function` is `arg`.

    By default, the range of codepoints passed to `function` includes all the characters in `charset`, but optional arguments `from-code` and `to-code` limit that to the range of characters between these two codepoints of `charset`. If either of them is `nil`, it defaults to the first or last codepoint of `charset`, respectively.

Next: [Scanning Charsets](Scanning-Charsets.html), Previous: [Character Properties](Character-Properties.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
