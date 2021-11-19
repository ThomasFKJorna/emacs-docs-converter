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

Next: [Numeric Conversions](Numeric-Conversions.html), Previous: [Predicates on Numbers](Predicates-on-Numbers.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 3.4 Comparison of Numbers

To test numbers for numerical equality, you should normally use `=` instead of non-numeric comparison predicates like `eq`, `eql` and `equal`. Distinct floating-point and large integer objects can be numerically equal. If you use `eq` to compare them, you test whether they are the same *object*; if you use `eql` or `equal`, you test whether their values are *indistinguishable*. In contrast, `=` uses numeric comparison, and sometimes returns `t` when a non-numeric comparison would return `nil` and vice versa. See [Float Basics](Float-Basics.html).

In Emacs Lisp, if two fixnums are numerically equal, they are the same Lisp object. That is, `eq` is equivalent to `=` on fixnums. It is sometimes convenient to use `eq` for comparing an unknown value with a fixnum, because `eq` does not report an error if the unknown value is not a number—it accepts arguments of any type. By contrast, `=` signals an error if the arguments are not numbers or markers. However, it is better programming practice to use `=` if you can, even for comparing integers.

Sometimes it is useful to compare numbers with `eql` or `equal`, which treat two numbers as equal if they have the same data type (both integers, or both floating point) and the same value. By contrast, `=` can treat an integer and a floating-point number as equal. See [Equality Predicates](Equality-Predicates.html).

There is another wrinkle: because floating-point arithmetic is not exact, it is often a bad idea to check for equality of floating-point values. Usually it is better to test for approximate equality. Here’s a function to do this:

    (defvar fuzz-factor 1.0e-6)
    (defun approx-equal (x y)
      (or (= x y)
          (< (/ (abs (- x y))
                (max (abs x) (abs y)))
             fuzz-factor)))

*   Function: **=** *number-or-marker \&rest number-or-markers*

    This function tests whether all its arguments are numerically equal, and returns `t` if so, `nil` otherwise.

<!---->

*   Function: **eql** *value1 value2*

    This function acts like `eq` except when both arguments are numbers. It compares numbers by type and numeric value, so that `(eql 1.0 1)` returns `nil`, but `(eql 1.0 1.0)` and `(eql 1 1)` both return `t`. This can be used to compare large integers as well as small ones.

<!---->

*   Function: **/=** *number-or-marker1 number-or-marker2*

    This function tests whether its arguments are numerically equal, and returns `t` if they are not, and `nil` if they are.

<!---->

*   Function: **<** *number-or-marker \&rest number-or-markers*

    This function tests whether each argument is strictly less than the following argument. It returns `t` if so, `nil` otherwise.

<!---->

*   Function: **<=** *number-or-marker \&rest number-or-markers*

    This function tests whether each argument is less than or equal to the following argument. It returns `t` if so, `nil` otherwise.

<!---->

*   Function: **>** *number-or-marker \&rest number-or-markers*

    This function tests whether each argument is strictly greater than the following argument. It returns `t` if so, `nil` otherwise.

<!---->

*   Function: **>=** *number-or-marker \&rest number-or-markers*

    This function tests whether each argument is greater than or equal to the following argument. It returns `t` if so, `nil` otherwise.

<!---->

*   Function: **max** *number-or-marker \&rest numbers-or-markers*

    This function returns the largest of its arguments.

        (max 20)
             ⇒ 20
        (max 1 2.5)
             ⇒ 2.5
        (max 1 3 2.5)
             ⇒ 3

<!---->

*   Function: **min** *number-or-marker \&rest numbers-or-markers*

    This function returns the smallest of its arguments.

        (min -4 1)
             ⇒ -4

<!---->

*   Function: **abs** *number*

    This function returns the absolute value of `number`.

Next: [Numeric Conversions](Numeric-Conversions.html), Previous: [Predicates on Numbers](Predicates-on-Numbers.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
