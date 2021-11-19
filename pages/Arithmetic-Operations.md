

Next: [Rounding Operations](Rounding-Operations.html), Previous: [Numeric Conversions](Numeric-Conversions.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 3.6 Arithmetic Operations

Emacs Lisp provides the traditional four arithmetic operations (addition, subtraction, multiplication, and division), as well as remainder and modulus functions, and functions to add or subtract 1. Except for `%`, each of these functions accepts both integer and floating-point arguments, and returns a floating-point number if any argument is floating point.

*   Function: **1+** *number-or-marker*

    This function returns `number-or-marker` plus 1. For example,

    ```lisp
    (setq foo 4)
         ⇒ 4
    (1+ foo)
         ⇒ 5
    ```

    This function is not analogous to the C operator `++`—it does not increment a variable. It just computes a sum. Thus, if we continue,

    ```lisp
    foo
         ⇒ 4
    ```

    If you want to increment the variable, you must use `setq`, like this:

    ```lisp
    (setq foo (1+ foo))
         ⇒ 5
    ```

<!---->

*   Function: **1-** *number-or-marker*

    This function returns `number-or-marker` minus 1.

<!---->

*   Function: **+** *\&rest numbers-or-markers*

    This function adds its arguments together. When given no arguments, `+` returns 0.

    ```lisp
    (+)
         ⇒ 0
    (+ 1)
         ⇒ 1
    (+ 1 2 3 4)
         ⇒ 10
    ```

<!---->

*   Function: **-** *\&optional number-or-marker \&rest more-numbers-or-markers*

    The `-` function serves two purposes: negation and subtraction. When `-` has a single argument, the value is the negative of the argument. When there are multiple arguments, `-` subtracts each of the `more-numbers-or-markers` from `number-or-marker`, cumulatively. If there are no arguments, the result is 0.

    ```lisp
    (- 10 1 2 3 4)
         ⇒ 0
    (- 10)
         ⇒ -10
    (-)
         ⇒ 0
    ```

<!---->

*   Function: **\*** *\&rest numbers-or-markers*

    This function multiplies its arguments together, and returns the product. When given no arguments, `*` returns 1.

    ```lisp
    (*)
         ⇒ 1
    (* 1)
         ⇒ 1
    (* 1 2 3 4)
         ⇒ 24
    ```

<!---->

*   Function: **/** *number \&rest divisors*

    With one or more `divisors`, this function divides `number` by each divisor in `divisors` in turn, and returns the quotient. With no `divisors`, this function returns 1/`number`, i.e., the multiplicative inverse of `number`. Each argument may be a number or a marker.

    If all the arguments are integers, the result is an integer, obtained by rounding the quotient towards zero after each division.

    ```lisp
    (/ 6 2)
         ⇒ 3
    ```

    ```lisp
    (/ 5 2)
         ⇒ 2
    ```

    ```lisp
    (/ 5.0 2)
         ⇒ 2.5
    ```

    ```lisp
    (/ 5 2.0)
         ⇒ 2.5
    ```

    ```lisp
    (/ 5.0 2.0)
         ⇒ 2.5
    ```

    ```lisp
    (/ 4.0)
         ⇒ 0.25
    ```

    ```lisp
    (/ 4)
         ⇒ 0
    ```

    ```lisp
    (/ 25 3 2)
         ⇒ 4
    ```

    ```lisp
    (/ -17 6)
         ⇒ -2
    ```

    If you divide an integer by the integer 0, Emacs signals an `arith-error` error (see [Errors](Errors.html)). Floating-point division of a nonzero number by zero yields either positive or negative infinity (see [Float Basics](Float-Basics.html)).

<!---->

*   Function: **%** *dividend divisor*

    This function returns the integer remainder after division of `dividend` by `divisor`. The arguments must be integers or markers.

    For any two integers `dividend` and `divisor`,

    ```lisp
    (+ (% dividend divisor)
       (* (/ dividend divisor) divisor))
    ```

    always equals `dividend` if `divisor` is nonzero.

    ```lisp
    (% 9 4)
         ⇒ 1
    (% -9 4)
         ⇒ -1
    (% 9 -4)
         ⇒ 1
    (% -9 -4)
         ⇒ -1
    ```

<!---->

*   Function: **mod** *dividend divisor*

    This function returns the value of `dividend` modulo `divisor`; in other words, the remainder after division of `dividend` by `divisor`, but with the same sign as `divisor`. The arguments must be numbers or markers.

    Unlike `%`, `mod` permits floating-point arguments; it rounds the quotient downward (towards minus infinity) to an integer, and uses that quotient to compute the remainder.

    If `divisor` is zero, `mod` signals an `arith-error` error if both arguments are integers, and returns a NaN otherwise.

    ```lisp
    (mod 9 4)
         ⇒ 1
    ```

    ```lisp
    (mod -9 4)
         ⇒ 3
    ```

    ```lisp
    (mod 9 -4)
         ⇒ -3
    ```

    ```lisp
    (mod -9 -4)
         ⇒ -1
    ```

    ```lisp
    (mod 5.5 2.5)
         ⇒ .5
    ```

    For any two numbers `dividend` and `divisor`,

    ```lisp
    (+ (mod dividend divisor)
       (* (floor dividend divisor) divisor))
    ```

    always equals `dividend`, subject to rounding error if either argument is floating point and to an `arith-error` if `dividend` is an integer and `divisor` is 0. For `floor`, see [Numeric Conversions](Numeric-Conversions.html).

Next: [Rounding Operations](Rounding-Operations.html), Previous: [Numeric Conversions](Numeric-Conversions.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
