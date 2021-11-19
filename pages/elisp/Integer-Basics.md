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

Next: [Float Basics](Float-Basics.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 3.1 Integer Basics

The Lisp reader reads an integer as a nonempty sequence of decimal digits with optional initial sign and optional final period.

     1               ; The integer 1.
     1.              ; The integer 1.
    +1               ; Also the integer 1.
    -1               ; The integer -1.
     0               ; The integer 0.
    -0               ; The integer 0.

The syntax for integers in bases other than 10 consists of ‘`#`’ followed by a radix indication followed by one or more digits. The radix indications are ‘`b`’ for binary, ‘`o`’ for octal, ‘`x`’ for hex, and ‘`radixr`’ for radix `radix`. Thus, ‘`#binteger`’ reads `integer` in binary, and ‘`#radixrinteger`’ reads `integer` in radix `radix`. Allowed values of `radix` run from 2 to 36, and allowed digits are the first `radix` characters taken from ‘`0`’–‘`9`’, ‘`A`’–‘`Z`’. Letter case is ignored and there is no initial sign or final period. For example:

    #b101100 ⇒ 44
    #o54 ⇒ 44
    #x2c ⇒ 44
    #24r1k ⇒ 44

To understand how various functions work on integers, especially the bitwise operators (see [Bitwise Operations](Bitwise-Operations.html)), it is often helpful to view the numbers in their binary form.

In binary, the decimal integer 5 looks like this:

    …000101

(The ellipsis ‘`…`’ stands for a conceptually infinite number of bits that match the leading bit; here, an infinite number of 0 bits. Later examples also use this ‘`…`’ notation.)

The integer -1 looks like this:

    …111111

\-1 is represented as all ones. (This is called *two’s complement* notation.)

Subtracting 4 from -1 returns the negative integer -5. In binary, the decimal integer 4 is 100. Consequently, -5 looks like this:

    …111011

Many of the functions described in this chapter accept markers for arguments in place of numbers. (See [Markers](Markers.html).) Since the actual arguments to such functions may be either numbers or markers, we often give these arguments the name `number-or-marker`. When the argument value is a marker, its position value is used and its buffer is ignored.

In Emacs Lisp, text characters are represented by integers. Any integer between zero and the value of `(max-char)`, inclusive, is considered to be valid as a character. See [Character Codes](Character-Codes.html).

Integers in Emacs Lisp are not limited to the machine word size. Under the hood, though, there are two kinds of integers: smaller ones, called *fixnums*, and larger ones, called *bignums*. Although Emacs Lisp code ordinarily should not depend on whether an integer is a fixnum or a bignum, older Emacs versions support only fixnums, some functions in Emacs still accept only fixnums, and older Emacs Lisp code may have trouble when given bignums. For example, while older Emacs Lisp code could safely compare integers for numeric equality with `eq`, the presence of bignums means that equality predicates like `eql` and `=` should now be used to compare integers.

The range of values for bignums is limited by the amount of main memory, by machine characteristics such as the size of the word used to represent a bignum’s exponent, and by the `integer-width` variable. These limits are typically much more generous than the limits for fixnums. A bignum is never numerically equal to a fixnum; Emacs always represents an integer in fixnum range as a fixnum, not a bignum.

The range of values for a fixnum depends on the machine. The minimum range is -536,870,912 to 536,870,911 (30 bits; i.e., -2\*\*29 to 2\*\*29 - 1), but many machines provide a wider range.

*   Variable: **most-positive-fixnum**

    The value of this variable is the greatest “small” integer that Emacs Lisp can handle. Typical values are 2\*\*29 - 1 on 32-bit and 2\*\*61 - 1 on 64-bit platforms.

<!---->

*   Variable: **most-negative-fixnum**

    The value of this variable is the numerically least “small” integer that Emacs Lisp can handle. It is negative. Typical values are -2\*\*29 on 32-bit and -2\*\*61 on 64-bit platforms.

<!---->

*   Variable: **integer-width**

    The value of this variable is a nonnegative integer that controls whether Emacs signals a range error when a large integer would be calculated. Integers with absolute values less than 2\*\*`n`, where `n` is this variable’s value, do not signal a range error. Attempts to create larger integers typically signal a range error, although there might be no signal if a larger integer can be created cheaply. Setting this variable to a large number can be costly if a computation creates huge integers.

Next: [Float Basics](Float-Basics.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
