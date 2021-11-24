

### 3.5 Numeric Conversions

To convert an integer to floating point, use the function `float`.

### Function: **float** *number*

This returns `number` converted to floating point. If `number` is already floating point, `float` returns it unchanged.

There are four functions to convert floating-point numbers to integers; they differ in how they round. All accept an argument `number` and an optional argument `divisor`. Both arguments may be integers or floating-point numbers. `divisor` may also be `nil`. If `divisor` is `nil` or omitted, these functions convert `number` to an integer, or return it unchanged if it already is an integer. If `divisor` is non-`nil`, they divide `number` by `divisor` and convert the result to an integer. If `divisor` is zero (whether integer or floating point), Emacs signals an `arith-error` error.

### Function: **truncate** *number \&optional divisor*

This returns `number`, converted to an integer by rounding towards zero.

```lisp
(truncate 1.2)
     ⇒ 1
(truncate 1.7)
     ⇒ 1
(truncate -1.2)
     ⇒ -1
(truncate -1.7)
     ⇒ -1
```

### Function: **floor** *number \&optional divisor*

This returns `number`, converted to an integer by rounding downward (towards negative infinity).

If `divisor` is specified, this uses the kind of division operation that corresponds to `mod`, rounding downward.

```lisp
(floor 1.2)
     ⇒ 1
(floor 1.7)
     ⇒ 1
(floor -1.2)
     ⇒ -2
(floor -1.7)
     ⇒ -2
(floor 5.99 3)
     ⇒ 1
```

### Function: **ceiling** *number \&optional divisor*

This returns `number`, converted to an integer by rounding upward (towards positive infinity).

```lisp
(ceiling 1.2)
     ⇒ 2
(ceiling 1.7)
     ⇒ 2
(ceiling -1.2)
     ⇒ -1
(ceiling -1.7)
     ⇒ -1
```

### Function: **round** *number \&optional divisor*

This returns `number`, converted to an integer by rounding towards the nearest integer. Rounding a value equidistant between two integers returns the even integer.

```lisp
(round 1.2)
     ⇒ 1
(round 1.7)
     ⇒ 2
(round -1.2)
     ⇒ -1
(round -1.7)
     ⇒ -2
```
