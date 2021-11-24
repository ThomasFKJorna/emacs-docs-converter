

### 3.9 Standard Mathematical Functions

These mathematical functions allow integers as well as floating-point numbers as arguments.

*   Function: **sin** *arg*
*   Function: **cos** *arg*
*   Function: **tan** *arg*

These are the basic trigonometric functions, with argument `arg` measured in radians.

### Function: **asin** *arg*

The value of `(asin arg)` is a number between -pi/2 and pi/2 (inclusive) whose sine is `arg`. If `arg` is out of range (outside \[-1, 1]), `asin` returns a NaN.

### Function: **acos** *arg*

The value of `(acos arg)` is a number between 0 and pi (inclusive) whose cosine is `arg`. If `arg` is out of range (outside \[-1, 1]), `acos` returns a NaN.

### Function: **atan** *y \&optional x*

The value of `(atan y)` is a number between -pi/2 and pi/2 (exclusive) whose tangent is `y`. If the optional second argument `x` is given, the value of `(atan y x)` is the angle in radians between the vector `[x, y]` and the `X` axis.

### Function: **exp** *arg*

This is the exponential function; it returns *e* to the power `arg`.

### Function: **log** *arg \&optional base*

This function returns the logarithm of `arg`, with base `base`. If you don’t specify `base`, the natural base *e* is used. If `arg` or `base` is negative, `log` returns a NaN.

### Function: **expt** *x y*

This function returns `x` raised to power `y`. If both arguments are integers and `y` is nonnegative, the result is an integer; in this case, overflow signals an error, so watch out. If `x` is a finite negative number and `y` is a finite non-integer, `expt` returns a NaN.

### Function: **sqrt** *arg*

This returns the square root of `arg`. If `arg` is finite and less than zero, `sqrt` returns a NaN.

In addition, Emacs defines the following common mathematical constants:

### Variable: **float-e**

The mathematical constant *e* (2.71828…).

### Variable: **float-pi**

The mathematical constant *pi* (3.14159…).
