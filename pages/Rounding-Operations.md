

Next: [Bitwise Operations](Bitwise-Operations.html), Previous: [Arithmetic Operations](Arithmetic-Operations.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 3.7 Rounding Operations

The functions `ffloor`, `fceiling`, `fround`, and `ftruncate` take a floating-point argument and return a floating-point result whose value is a nearby integer. `ffloor` returns the nearest integer below; `fceiling`, the nearest integer above; `ftruncate`, the nearest integer in the direction towards zero; `fround`, the nearest integer.

*   Function: **ffloor** *float*

    This function rounds `float` to the next lower integral value, and returns that value as a floating-point number.

<!---->

*   Function: **fceiling** *float*

    This function rounds `float` to the next higher integral value, and returns that value as a floating-point number.

<!---->

*   Function: **ftruncate** *float*

    This function rounds `float` towards zero to an integral value, and returns that value as a floating-point number.

<!---->

*   Function: **fround** *float*

    This function rounds `float` to the nearest integral value, and returns that value as a floating-point number. Rounding a value equidistant between two integers returns the even integer.
