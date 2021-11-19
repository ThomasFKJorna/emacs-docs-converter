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

Previous: [Syntax Class Table](Syntax-Class-Table.html), Up: [Syntax Descriptors](Syntax-Descriptors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 35.2.2 Syntax Flags

In addition to the classes, entries for characters in a syntax table can specify flags. There are eight possible flags, represented by the characters ‘`1`’, ‘`2`’, ‘`3`’, ‘`4`’, ‘`b`’, ‘`c`’, ‘`n`’, and ‘`p`’.

All the flags except ‘`p`’ are used to describe comment delimiters. The digit flags are used for comment delimiters made up of 2 characters. They indicate that a character can *also* be part of a comment sequence, in addition to the syntactic properties associated with its character class. The flags are independent of the class and each other for the sake of characters such as ‘`*`’ in C mode, which is a punctuation character, *and* the second character of a start-of-comment sequence (‘`/*`’), *and* the first character of an end-of-comment sequence (‘`*/`’). The flags ‘`b`’, ‘`c`’, and ‘`n`’ are used to qualify the corresponding comment delimiter.

Here is a table of the possible flags for a character `c`, and what they mean:

*   ‘`1`’ means `c` is the start of a two-character comment-start sequence.

*   ‘`2`’ means `c` is the second character of such a sequence.

*   ‘`3`’ means `c` is the start of a two-character comment-end sequence.

*   ‘`4`’ means `c` is the second character of such a sequence.

*   ‘`b`’ means that `c` as a comment delimiter belongs to the alternative “b” comment style. For a two-character comment starter, this flag is only significant on the second char, and for a 2-character comment ender it is only significant on the first char.

*   ‘`c`’ means that `c` as a comment delimiter belongs to the alternative “c” comment style. For a two-character comment delimiter, ‘`c`’ on either character makes it of style “c”.

*   ‘`n`’ on a comment delimiter character specifies that this kind of comment can be nested. Inside such a comment, only comments of the same style will be recognized. For a two-character comment delimiter, ‘`n`’ on either character makes it nestable.

    Emacs supports several comment styles simultaneously in any one syntax table. A comment style is a set of flags ‘`b`’, ‘`c`’, and ‘`n`’, so there can be up to 8 different comment styles, each one named by the set of its flags. Each comment delimiter has a style and only matches comment delimiters of the same style. Thus if a comment starts with the comment-start sequence of style “bn”, it will extend until the next matching comment-end sequence of style “bn”. When the set of flags has neither flag ‘`b`’ nor flag ‘`c`’ set, the resulting style is called the “a” style.

    The appropriate comment syntax settings for C++ can be as follows:

    *   ‘`/`’

        ‘`124`’

    *   ‘`*`’

        ‘`23b`’

    *   newline

        ‘`>`’

    This defines four comment-delimiting sequences:

    *   ‘`/*`’

        This is a comment-start sequence for “b” style because the second character, ‘`*`’, has the ‘`b`’ flag.

    *   ‘`//`’

        This is a comment-start sequence for “a” style because the second character, ‘`/`’, does not have the ‘`b`’ flag.

    *   ‘`*/`’

        This is a comment-end sequence for “b” style because the first character, ‘`*`’, has the ‘`b`’ flag.

    *   newline

        This is a comment-end sequence for “a” style, because the newline character does not have the ‘`b`’ flag.

*   ‘`p`’ identifies an additional prefix character for Lisp syntax. These characters are treated as whitespace when they appear between expressions. When they appear within an expression, they are handled according to their usual syntax classes.

    The function `backward-prefix-chars` moves back over these characters, as well as over characters whose primary syntax class is prefix (‘`'`’). See [Motion and Syntax](Motion-and-Syntax.html).

Previous: [Syntax Class Table](Syntax-Class-Table.html), Up: [Syntax Descriptors](Syntax-Descriptors.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
