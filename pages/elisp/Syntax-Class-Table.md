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

Next: [Syntax Flags](Syntax-Flags.html), Up: [Syntax Descriptors](Syntax-Descriptors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 35.2.1 Table of Syntax Classes

Here is a table of syntax classes, the characters that designate them, their meanings, and examples of their use.

*   Whitespace characters: ‘` `’ or ‘`-`’

    Characters that separate symbols and words from each other. Typically, whitespace characters have no other syntactic significance, and multiple whitespace characters are syntactically equivalent to a single one. Space, tab, and formfeed are classified as whitespace in almost all major modes.

    This syntax class can be designated by either ‘` `’<!-- /@w --> or ‘`-`’. Both designators are equivalent.

*   Word constituents: ‘`w`’

    Parts of words in human languages. These are typically used in variable and command names in programs. All upper- and lower-case letters, and the digits, are typically word constituents.

*   Symbol constituents: ‘`_`’

    Extra characters used in variable and command names along with word constituents. Examples include the characters ‘`$&*+-_<>`’ in Lisp mode, which may be part of a symbol name even though they are not part of English words. In standard C, the only non-word-constituent character that is valid in symbols is underscore (‘`_`’).

*   Punctuation characters: ‘`.`’

    Characters used as punctuation in a human language, or used in a programming language to separate symbols from one another. Some programming language modes, such as Emacs Lisp mode, have no characters in this class since the few characters that are not symbol or word constituents all have other uses. Other programming language modes, such as C mode, use punctuation syntax for operators.

*   *   Open parenthesis characters: ‘`(`’
    *   Close parenthesis characters: ‘`)`’

    Characters used in dissimilar pairs to surround sentences or expressions. Such a grouping is begun with an open parenthesis character and terminated with a close. Each open parenthesis character matches a particular close parenthesis character, and vice versa. Normally, Emacs indicates momentarily the matching open parenthesis when you insert a close parenthesis. See [Blinking](Blinking.html).

    In human languages, and in C code, the parenthesis pairs are ‘`()`’, ‘`[]`’, and ‘`{}`’. In Emacs Lisp, the delimiters for lists and vectors (‘`()`’ and ‘`[]`’) are classified as parenthesis characters.

*   String quotes: ‘`"`’

    Characters used to delimit string constants. The same string quote character appears at the beginning and the end of a string. Such quoted strings do not nest.

    The parsing facilities of Emacs consider a string as a single token. The usual syntactic meanings of the characters in the string are suppressed.

    The Lisp modes have two string quote characters: double-quote (‘`"`’) and vertical bar (‘`|`’). ‘`|`’ is not used in Emacs Lisp, but it is used in Common Lisp. C also has two string quote characters: double-quote for strings, and apostrophe (‘`'`’) for character constants.

    Human text has no string quote characters. We do not want quotation marks to turn off the usual syntactic properties of other characters in the quotation.

*   Escape-syntax characters: ‘`\`’

    Characters that start an escape sequence, such as is used in string and character constants. The character ‘`\`’ belongs to this class in both C and Lisp. (In C, it is used thus only inside strings, but it turns out to cause no trouble to treat it this way throughout C code.)

    Characters in this class count as part of words if `words-include-escapes` is non-`nil`. See [Word Motion](Word-Motion.html).

*   Character quotes: ‘`/`’

    Characters used to quote the following character so that it loses its normal syntactic meaning. This differs from an escape character in that only the character immediately following is ever affected.

    Characters in this class count as part of words if `words-include-escapes` is non-`nil`. See [Word Motion](Word-Motion.html).

    This class is used for backslash in TeX mode.

*   Paired delimiters: ‘`$`’

    Similar to string quote characters, except that the syntactic properties of the characters between the delimiters are not suppressed. Only TeX mode uses a paired delimiter presently—the ‘`$`’ that both enters and leaves math mode.

*   Expression prefixes: ‘`'`’

    Characters used for syntactic operators that are considered as part of an expression if they appear next to one. In Lisp modes, these characters include the apostrophe, ‘`'`’ (used for quoting), the comma, ‘`,`’ (used in macros), and ‘`#`’ (used in the read syntax for certain data types).

*   *   Comment starters: ‘`<`’
    *   Comment enders: ‘`>`’

    Characters used in various languages to delimit comments. Human text has no comment characters. In Lisp, the semicolon (‘`;`’) starts a comment and a newline or formfeed ends one.

*   Inherit standard syntax: ‘`@`’

    This syntax class does not specify a particular syntax. It says to look in the standard syntax table to find the syntax of this character.

*   Generic comment delimiters: ‘`!`’

    (This syntax class is also known as “comment-fence”.) Characters that start or end a special kind of comment. *Any* generic comment delimiter matches *any* generic comment delimiter, but they cannot match a comment starter or comment ender; generic comment delimiters can only match each other.

    This syntax class is primarily meant for use with the `syntax-table` text property (see [Syntax Properties](Syntax-Properties.html)). You can mark any range of characters as forming a comment, by giving the first and last characters of the range `syntax-table` properties identifying them as generic comment delimiters.

*   Generic string delimiters: ‘`|`’

    (This syntax class is also known as “string-fence”.) Characters that start or end a string. This class differs from the string quote class in that *any* generic string delimiter can match any other generic string delimiter; but they do not match ordinary string quote characters.

    This syntax class is primarily meant for use with the `syntax-table` text property (see [Syntax Properties](Syntax-Properties.html)). You can mark any range of characters as forming a string constant, by giving the first and last characters of the range `syntax-table` properties identifying them as generic string delimiters.

Next: [Syntax Flags](Syntax-Flags.html), Up: [Syntax Descriptors](Syntax-Descriptors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
