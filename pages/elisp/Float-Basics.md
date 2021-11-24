

### 3.2 Floating-Point Basics

Floating-point numbers are useful for representing numbers that are not integral. The range of floating-point numbers is the same as the range of the C data type `double` on the machine you are using. On all computers supported by Emacs, this is IEEE binary64 floating point format, which is standardized by [IEEE Std 754-2019](https://standards.ieee.org/standard/754-2019.html) and is discussed further in David Goldberg’s paper “[What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)”. On modern platforms, floating-point operations follow the IEEE-754 standard closely; however, results are not always rounded correctly on some obsolescent platforms, notably 32-bit x86.

The read syntax for floating-point numbers requires either a decimal point, an exponent, or both. Optional signs (‘`+`’ or ‘`-`’) precede the number and its exponent. For example, ‘`1500.0`’, ‘`+15e2`’, ‘`15.0e+2`’, ‘`+1500000e-3`’, and ‘`.15e4`’ are five ways of writing a floating-point number whose value is 1500. They are all equivalent. Like Common Lisp, Emacs Lisp requires at least one digit after any decimal point in a floating-point number; ‘`1500.`’ is an integer, not a floating-point number.

Emacs Lisp treats `-0.0` as numerically equal to ordinary zero with respect to numeric comparisons like `=`. This follows the IEEE floating-point standard, which says `-0.0` and `0.0` are numerically equal even though other operations can distinguish them.

The IEEE floating-point standard supports positive infinity and negative infinity as floating-point values. It also provides for a class of values called NaN, or “not a number”; numerical functions return such values in cases where there is no correct answer. For example, `(/ 0.0 0.0)` returns a NaN. A NaN is never numerically equal to any value, not even to itself. NaNs carry a sign and a significand, and non-numeric functions treat two NaNs as equal when their signs and significands agree. Significands of NaNs are machine-dependent, as are the digits in their string representation.

When NaNs and signed zeros are involved, non-numeric functions like `eql`, `equal`, `sxhash-eql`, `sxhash-equal` and `gethash` determine whether values are indistinguishable, not whether they are numerically equal. For example, when `x` and `y` are the same NaN, `(equal x y)` returns `t` whereas `(= x y)` uses numeric comparison and returns `nil`; conversely, `(equal 0.0 -0.0)` returns `nil` whereas `(= 0.0 -0.0)` returns `t`.

Here are read syntaxes for these special floating-point values:

infinity

‘`1.0e+INF`’ and ‘`-1.0e+INF`’

not-a-number

‘`0.0e+NaN`’ and ‘`-0.0e+NaN`’

The following functions are specialized for handling floating-point numbers:

### Function: **isnan** *x*

This predicate returns `t` if its floating-point argument is a NaN, `nil` otherwise.

### Function: **frexp** *x*

This function returns a cons cell `(s . e)`, where `s` and `e` are respectively the significand and exponent of the floating-point number `x`.

If `x` is finite, then `s` is a floating-point number between 0.5 (inclusive) and 1.0 (exclusive), `e` is an integer, and `x` = `s` \* 2\*\*`e`. If `x` is zero or infinity, then `s` is the same as `x`. If `x` is a NaN, then `s` is also a NaN. If `x` is zero, then `e` is 0.

### Function: **ldexp** *s e*

Given a numeric significand `s` and an integer exponent `e`, this function returns the floating point number `s` \* 2\*\*`e`.

### Function: **copysign** *x1 x2*

This function copies the sign of `x2` to the value of `x1`, and returns the result. `x1` and `x2` must be floating point.

### Function: **logb** *x*

This function returns the binary exponent of `x`. More precisely, if `x` is finite and nonzero, the value is the logarithm base 2 of *|x|*, rounded down to an integer. If `x` is zero or infinite, the value is infinity; if `x` is a NaN, the value is a NaN.

```lisp
(logb 10)
     ⇒ 3
(logb 10.0e20)
     ⇒ 69
(logb 0)
     ⇒ -1.0e+INF
```
